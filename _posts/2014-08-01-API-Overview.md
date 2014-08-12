# API overview

## Endpoint

The entry point to the API is:   
**Production:** https://loyal.guru/api    
**Staging:** http://loyalty-jobtitude-staging.herokuapp.com/api/   

## Data Types

The data types of the fields returned by any API call can be:

- **String**: when in JSON, it will be enclosed in double quotes.
Number: The decimal separator, if any, will be always the dot character ".".
- **Datetime**: represented as UTC Strings in [ISO-8601](http://en.wikipedia.org/wiki/ISO_8601) as [Coordinated Universal Time](http://en.wikipedia.org/wiki/Offset_to_Coordinated_Universal_Time) (UTC) with zero offset. Example: 2013-12-18T01:46:58Z.
- **Boolean**: represented as the literal true or false (without quotes)
- **Array**: when in JSON, it will follow the canonical [] representation. When in XML, an array will be sent as a list of several elements with the same tag name

## Formats

The response format of the API at the moment is only JSON.

## Navigating the API

Loyal Guru API provides hypermedia links in every response. In the same way when you visit a web page you can browse around simply by clicking links, in our API you can navigate from one request to the next just following the provided links.

We strongly recommend the use of hypermedia links for accessing API resources. Composing URLs by concatenating the endpoint and id of a resource manually shouldn't be done, as we might decide to change our URL schema in the future and your client might stop working.

Pagination is also done via hyperlinks. Please read the API Hypermedia section for more information.

You can find an example if you point your browser to base_api_url/public

## GUID's

Always will use GUID's except when calling as a USER asking for a CUSTOMER. In that case we'll ask for the <code>memberhip_number</code>, you can configure this param in order to be the DNI of the person, an incremental number, etc.. ) 

## Conditional requests
Most API responses come with appropriate cache-control headers set to assist in client-side caching. If you have cached a response, do not request again until the response has expired; when you do request something again, set the **If-Modified-Since** request header to the “last modified” time returned. If the response hasn’t changed, the Loyal Guru service will respond quickly with the 304 Not Modified status (i.e., your cached version is still good and your application should use it)

GET requests to the certains endpoints ( private resource ) return an etag in the response header. The etag can be sent in a **If-None-Match** header in subsequent requests to the same endpoint for the same resource. If the resource has not changed, the Loyal Guru service will respond quickly with the 304 Not Modified status indicating that your cached version is still valid.

Take a look [here](https://devcenter.heroku.com/articles/increasing-application-performance-with-http-cache-headers) to understand HTTP Cache