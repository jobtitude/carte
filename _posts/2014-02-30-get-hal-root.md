---
category: Endpoints
path: '/public'
title: 'Get Api HAL Root'
type: 'GET'

layout: nil
---

This method allows the get the HAL root in order to traverse the APi

### Request

* That's the onlye call, tha no matter what headers you specify will work.

### Response

Sends back a collection of links, templated or not, in order to traverse the API withoud the need of build manually the urls.

```Status: 200 ```

{% highlight json %}
{
    "_links":
        {
            "sign_in":{"href":"http://localhost:3000/api/customers/sign_in"},
            "sign_up":{"href":"http://localhost:3000/api/customers"},
            "password_reset":{"href":"http://localhost:3000/api/customers/password"},
            "profile":{
                "href":"http://localhost:3000/api/customers/{id}",
                "templated":true},
            }
            ...
        }
}
{% endhighlight %}






