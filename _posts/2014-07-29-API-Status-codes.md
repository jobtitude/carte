# API Status Codes

Together with the response for each request, our API always sends a meaningful HTTP status code.
**No need to specify number, stick to <code>:status</code> from rails <code>:ok</code>, <code>:created</code> and many more**:
http://guides.rubyonrails.org/layouts_and_rendering.html#the-status-option

**Status codes in the <code>40x</code> and <code>50x</code> ranges will provide a <code>message</code> field with a description of the particular error**, optinonally depending on the error will also supply an **errors** array

Common error response
```json
{
    "message": "Some sort of error message",
}
```

Enriched error response
```json
{
  "message": "Some sort of complete error message",
  "errors":{
    "name": ["this field can't be blank", "this field can't be number"],
    "customer_id":["This field launches red bubbles"]
   },
}
```
## 200 OK

The request was completed successfully. You may see this status after a GET, PUT, PATCH or DELETE.

## 201 Created

The request was completed successfully and a new resource was created. You will see this status only after a POST.

The returned object will typically have a hypermedia link named self with the URI of the new resource and more complementary data.

## 202 Accepted

The request was completed successfully, but there is a pending task to do before the new resource can be used. You will see this status after a POST.

At the moment, we are only using this status after uploading a new avatar, while we are generating thumbnails on the server. The response will have a hypermedia link named status indicating the progress of the task.

If you need to use the new resource right away, you can issue a GET to the self hypermedia link after a few seconds to check if the operation has already finished.

## 301 Moved Permanently

The requested resource has been moved to a different URI, and your HTTP client should retry the request against the new one.

## 304 Not Modified

If your client support Etags and you issue a GET that would return the same resource that was indicated by the Etag, we will return this header. ( Take a look at Conditional Requests in API Overview )

## 400 Bad request

You are trying to log in and your credentials are invalid, or you are trying to authenticate with a invalid OAuth2 token.

Also, you are issuing a request we can't accept. The most likely reason is you are not sending JSON or XML, or you are not sending an accept-header with the appropriate mime type. Or may be other parameters are wrong.

## 401 Unauthorized

You are trying to log in and your credentials are invalid, or you are trying to authenticate with a invalid OAuth2 token.

note: If you get a 401 when signing in, you won't see an errors field in the response. The API will send an error and an error_description field instead. The reason for this is the OAuth2 protocol uses those standard fields, and by using them we allow any OAuth2 compliant client to log in into our system.

## 403 Forbidden

You are not logged in, and the requested resource is not public, or you are logged in but you don't have access to the requested resource.

## 404 Not Found

The requested resource couldn't be found.

## 422 Unprocessable Entity

There was an error processing your request. You will find more information in the errors field. The most likely causes are validation errors or trying to create duplicated resources.

## 502  Bad Gateway 

The server was acting as a gateway or proxy and received an invalid response from the upstream server.

## 503 Service Unavailable 

The server is currently unable to handle the request due to a temporary condition which will be alleviated after some delay. You can choose to resend the request again.

