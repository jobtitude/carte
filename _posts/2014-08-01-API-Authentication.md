## Getting your access token

By the moment the only way to get an access token is to send us an e-mail to info@loyal.guru.

## Using your access token
Cada llamada realizada deberá llevar una cabecera para autenticarse, en este caso se trata 
de una cabecera básica HTTP AUTH que se compone de un token y un usuario ( que 
variaran en cada location y empresa ).
Esta llamada se ha de componer con un encode en base 64 y con un caracter : que separa 
el usuario y el token, ejemplo:  
```Authorization="Basic base64(prueba@jobtitude.com:user-2AAAAAA9dZdzFV4HA)"```

## User roles

Loyal Guru is a **multi-role API**. The API can be used as a USER and as a CUSTOMER. The same calls depending the role will issue different ERROS.
- **USER**: Shop owner, tpv, shop client app
- **CUSTOMER**: normal person, web client, app client

***

### Pendiente: Oauth 2

Sólo tiene sentido para permitir:
- el login en tu aplicación por parte de otra aplicación ( como cuando twitter permite vincular tu cuenta de X servicio para que puedas enviar un twitt directamente, o cualquier otra integración )
- el login en una aplicación a traves de la tuya ( como en asana permiten logarte usando el login de google )
- En general es para integraciones con terceros. Puede tener sentido para integrar con NETIP, o quizás, pero no seguro, con los TPV's.
- La gema ideal para esto es **DoorKeeper**