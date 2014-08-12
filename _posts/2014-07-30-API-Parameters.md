# API Parameters

Our API allows a few parameters to modify the behaviour of the server when composing the response. As an example, let's execute the request below to see the profile of the user Ada

## General

Many API methods take optional parameters. For **GET requests**, any parameters not specified as a segment in the path can be passed as an HTTP query string parameter:

```bash
$ curl -i "https://loyal.guru/api/customers/12/vouchers?status=expired"
```

In this example, the ‘12’ value is provided for the :customer_id parameter in the path while :status is passed in the query string.

For **POST, PATCH, PUT, and DELETE requests**, parameters not included in the URL should be encoded as JSON with a **Content-Type of ‘application/json’**:

```bash
$ curl -i -u username -d '{"customer_id":12, "total":200,"line_attributes":[{"product_id":1},{"product_id":123}]}' https://loyal.guru/api/activities
```


## Versioning

Version the API from the start. Use the Accepts header to communicate the version, along with a custom content type, e.g.:

<code>Accept: application/vnd.loyalguru.v1</code>

We have as default version always the last version, but please require clients to explicitly peg their usage to a specific version.

We'll hope in a near future to find a way to avoid versioning thanks to a strong Hypermedia usage, with link discovering functionality.

## Pagination

Pagination is controlled by hypermedia in our API. You shouldn't try to compose the pagination links by yourself, but use instead the <code>self(start)</code>, <code>next</code>, and <code>previous</code> hyperlinks.

***

**Pending**: 

##Pagination
The api parameters controlling pagination limits are <code>list_to</code> and <code>list_since</code>. The semantics of the values depend on the particular resources, so it's highly advisable not to tamper these values and use directly the hyperlinks provided.
If you want to get a number of items per page different to the default (usually 15), you can pass the parameter <code>list_count</code> set to the desired amount.

Take a look at how Spotify Web Api is solving [this](https://developer.spotify.com/web-api/object-model/#paging-object).


##Embedded Resources

To get embedded resources, you need to pass the parameter <code>api_embed</code> set to the name of the resource to embed. If you want to embed different resources, you can pass a comma separated list of values.

## Hyperlinks

By default, we send hypermedia links together with the response. If you don't want to get the hyperlinks for a particular request, just set the parameter <code>api_links</code> to false