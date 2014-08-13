---
category: ejemplos
title: 'Integración TPV'

layout: nil
---


# Ejemplo básico de integración Loyal Guru <-> TPV

**En resumen:**

A continuación se presenta, y detalla paso a paso, un escensario sencillo de integración de la plataforma Loyal Guru con un sistema de TPV en un establecimiento. Imaginemos un escenario en el que el cliente se identifica ante el vendedor, presenta un cupón de descuento, se aplica un descuento sobre su compra y se finaliza la compra. En este proceso se realizan 3 llamadas al sistema de Loyal Guru ( 1. Identificación de cliente 2. Validación de cupón 3. Guardar la compra realizada ).

**En detalle:**

El consumidor dispone de una aplicación móvil ( iOs/Android ) en la que se ha registrado con sus datos personales, al llegar al punto de venta se identifica en el TPV enseñando una pantalla de la aplicación en la que aparece un código alfanumérico ( que puede leerse visualmente o escanearse en un QR), desde el TPV una vez introducido el número de cliente leído se hace una llamada al sistema de Loyal Guru para obtener la información del cliente y mostrarla en el TPV ( nombre, compras realiazdas, etc.. ) , una vez completada la compra el TPV enviará los datos de la compra y del cliente a la plataforma de Loyal Guru mediante otra llamada.

El cliente pueda presentar en el momento de la compra un cupón de descuento enseñando otra pantalla de la aplicación móvil en la que aparecerá un código único del cupón ( que podrá leerse o escanearse de un QR ), éste se deberá introducir en el TPV y entonces mediante un botón en el tpv de "validar" se deberá realizar una llamada a Loyal Guru para comprobar que el código de cupón pertenece a un cupón válido y para además obtener que tipo de descuento tiene asociado el cupón en cuestión ( inicialmente X% sobre el total de la compra ). Una vez validado el cupón desde el TPV se podrá aplicar directamente el descuento asociado. Será importante que si a la compra se le ha aplicado un descuento vía cupón, el id del cupón tambiérn deberá enviarse al guardar la compra junto a los datos del cliente y los datos de la compra.

**Detalles técnicos para llamar a la API**: 

- La api se basa en el [protocolo HTTP](http://code.tutsplus.com/tutorials/http-the-protocol-every-web-developer-must-know-part-1--net-31177), conviene entender el proceso y la estructura de una **petición** y de una **respuesta** en este protocolo.  
- Los ejemplos de llamada se ilustran con [curl](http://curl.haxx.se/) pero el desarrollador deberá utilizar un **cliente HTTP** en función del lenguaje de desarrollo ( ej: [Faraday](https://github.com/lostisland/faraday)-Ruby, [HTTPFull](http://phphttpclient.com/)-PHP, [RestSharp](http://restsharp.org/)-.Net, etc.. ).   
- La **url base de la api** también deberá especificarse de forma correcta ( este ejemplo es del entorno de TEST: http://loyalty-jobtitude-staging.herokuapp.com/api , en PRODUCTION sería https://loyal.guru/api ).  
- Es importante recordar que las llamadas tendrán que realizarse **indicando de forma correcta las cabeceras en la petición**. La cabecera de  **autenticación** ( en el ejemplo -u ), la de **accept** para la versión de la api ( en el ejemplo -H ), la de **content-ype** ( en el ejemplo -H ), sino los resultados serán incorrectos. 
- Y para interactuar con la api será necesario interpretar tanto el **cuerpo de las respuestas** ( para el contenido de las respuestas ) como las **cabeceras** ( status de error, etc.. ).

## Detalle técnico de las 3 llamadas

### 1. Identificar al cliente vía su código único

1. Habilitamos **campo** en pantalla de compra de TPV de _código cliente_, y (opcionalmente) **botón** de "identificar"
2. Escaneamos el qr o leemos el código directamente de la aplicación móvil del ciente y lo **introducimos** en este campo.
3. Al introducir el código ( o presionar botón de "identificar" ) se realiza llamada con el método **GET** al sistema de loyal guru con la siguiente estructura ( notar el id en ..customer/**1**.. y el parametro **with_html**):  

**petición** a la API:    

  {% highlight shell %}
curl http://loyalty-jobtitude-staging.herokuapp.com/api/customers/1\?with_html\=true 
   -u prueba@jobtitude.com:user-2HZDS1rAKn9dZdzFV4HA 
   -H "Content-Type:application/json" 
   -H "Accept: application/vnd.loyalguru.v1"
{% endhighlight %} 

**ÉXITO**: Si la llamada devuelve un **status de 200** significará que el **consumidor se ha encontrado**, y el cuerpo de la respuesta vendrá con los datos del cliente y además con un campo "html" que podrá mostrarse directamente en el espacio deseado del tpv ( con el nombre, los puntos del cliente, etc... )  

**cabeceras** de la respuesta:  

{% highlight shell %}
Host: loyalty-jobtitude-staging.herokuapp.com
> Content-Type:application/json
> Accept: application/vnd.loyalguru.v1
>
< HTTP/1.1 200 OK
* Server Cowboy is not blacklisted
< Server: Cowboy
< Connection: close
< Date: Tue, 12 Aug 2014
...
{% endhighlight %} 

**cuerpo** de la respuesta:
{% highlight json %}
{
  "html": "\n        <p>Eric Ponce Rodriguez</p>\n        <p>0</p>\n      ",
  "membership_number": 1,
  "id": 3,
  "name": "Eric",
  "surname_1": "Ponce",
  "surname_2": "Rodriguez",
  "email": "eponce+cliente@jobtitude.com",
  "score_available": 0,
  "score": 0
  ...
}
{% endhighlight %}

 **ERROR - CONSUMIDOR NO ENCONTRADO:** Si la llamada devuelve un **status de error 404** ( not found ) significará que el **consumidor no existe**, mostaremos por pantalla el mensaje necesaio ( ej: Cliente no registrado en el sistema, por favor recomendar registro en aplicación móvil o introducir número correcto )  

**cabeceras** de la respuesta:
{% highlight bash %}
> User-Agent: curl/7.30.0
> Host: loyalty-jobtitude-staging.herokuapp.com
> Content-Type:application/json
> Accept: application/vnd.loyalguru.v1
>
< HTTP/1.1 404 Not Found
* Server Cowboy is not blacklisted
< Server: Cowboy
< Connection: close
< Date: Tue, 12 Aug 2014 15:16:14 GMT
...
{% endhighlight %}

**ERROR - cuatrocientos:** Es posible que la API devuelva otros errores ( 400 : petición mal realiazada , 403 : acción prohibida , etc.. ) recomendamos consultar la [lista de status http](http://en.wikipedia.org/wiki/List_of_HTTP_status_codes) en caso de duda, o enviar un mail a developers@loyal.guru. 


### 2. Validar cupón

1. **Habilitamos campo** de "cupón/promoción" en pantalla de compra de tpv, y ( opcionalmente) **botón** de "validar".
2. Escaneamos QR o leemos código de cupón directamente de la aplicación y lo **introducimos** en el campo creado
3. Al introducir el código ( o presionar botón de "validar" ) se **realiza llamada** con el método **GET** al sistema de loyal guru con la siguiente estructura:
{% highlight shell %}
curl http://loyalty-jobtitude-staging.herokuapp.com/api/vouchers/1 
   -u prueba@jobtitude.com:user-2HZDS1rAKn9dZdzFV4HA 
   -H "Content-Type:application/json" 
   -H "Accept: application/vnd.loyalguru.v1"
{% endhighlight %}  

**ERROR:** Si la llamada devuelve un **status de ERROR 404** ( not found ) significará que el **cupón no existe**, mostaremos por pantalla el mensaje necesaio ( ej: Cupón no encontrado en el sistema, por favor recomendar la recarga del cupón o la introducción del número de cupón correcto )

**ÉXITO:** Si la llamada devuelve un **status de 200** significará que el **cupón se ha encontrado** y entonces en el cuerpo de la respuesta llegará un objeto con los datos del cupón.
  
* Si el **cupón ya ha sido utilizado** y por lo tanto no es válido el campo "**pending**" vendrá a "**false**", y por lo tanto se podrá mostrar un mensaje de "Lo sentimos el cupón ya se ha utilizado"  
* Si el **cupón es válido** y todavía no se ha canjeado el campo "**pending**" estará a "**true**" y entonces será necesario acceder al campo "discounts". Este campo será un array de objectos de descuentos en el que para el caso de por ejemplo un descuento de un 20% sobre el total del tickect llegará un en el objecto un campo de "**discount**":"**20%**". De esta forma, será responsabilidad del TPV coger este descuento ( ej: 20% ) y aplicarlo al total del ticket.  

### 3. Guardar la compra

1. Al cerrar la compra se realizará una llamada con el método **POST** al sistema de loyal guru:   

**petición** a la API: 

{% highlight bash %}
curl -v http://loyalty-jobtitude-staging.herokuapp.com/api/activities 
-u prueba@jobtitude.com:user-2HZDS1rAKn9dZdzFV4HA 
-H "Content-Type:application/json" 
-H "Accept: application/vnd.loyalguru.v1"  
-d '{
	"customer_id":"1", 
	"external_id":"200", 
	"total":"4000", 
	"location_id":"1",
	"voucher_id":"xxxxxxx", 
	"lines_attributes": [
		{"product_id":"99","quantity":"3","order":"1","price":"50"},
		{"product_id":"45678","quantity":"4","order":"2","price":"199"}
		]
}'
{% endhighlight %}

Notar: 

- que los **parámetros**, al especificar el content-type en **JSON** deberán enviarse en JSON. 
- **customer_id** sería el id escaneado del consumidor
- **external_id** sería el id del ticket en el sistema propio de la empresa
- **location_id** sería el id del establecimiento del tpv de la empresa ( otorgado por Loyal Guru )
- **total** sería el dinero total pagado por el cliente en ese ticket
- **lines_attributes** es el array que contiene cada producto ( **product_id**: id del producto para la empresa, **quantity**: cantidad de productos en ticket, **order**: orden de la línea de ticket, **price**: precio del artículo ) 
- **voucher_id**: sería el código del cupón validado  

***

# Consejos de implementación en TPV

- Funcionando **con Scanner de QR** es posible **obviar los botones de "identificar" cliente o "validar" cupón**, ya que el scanner se puede configurar para leer el QR y automaticamente realizar un "INTRO", este intro sería el que realizaría la llamada de identificación o validación.
- Que **al validar un cupón**, en el caso de que sea bueno y no usado,** que no se permita modificar ni borrar el campo de "cupón/promoción"** ( de esta forma se evitan situaciones en las que se valida cupón, se aplica descuento, el vendedor se quita el campo de cupón , se guarda compra con descuento aplicado y sin añadir el id de cupón, y entonces el cupón queda sin canjear pudiendose usar de nuevo )
- **Entornos**: Las pruebas se realizarán en el entorno de **staging** ( url base de la api: http://loyalty-jobtitude-staging.herokuapp.com/api ), mientras que una vez todo esté listo se pasará al entorno de **producción** ( url base de la api: http://loyal.guru/api )
- Recomendamos guardar un fichero de configuración con **url** base de la api, **location_id** del establecimiento, **user** y **token**. 