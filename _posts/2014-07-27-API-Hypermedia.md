# API Hypermedia

Recommended reading: http://stateless.co/hal_specification.html

## Link relations

Most responses from our API will contain a property named _links. This property is a JSON object, or in the XML version an element, with one entry for every link relation.

{% highlight json %}
"_links": {
  "link_categories": {
    "href": "https://loyal.guru/api/people/ada/link-categories"
  },
  "self": {
    "href": "https://loyal.guru/api/people/ada"
  },
  "user": {
    "href": "https://loyal.guru/api/users/ada"
  },
  "avatar_small": {
    "href": "https://s3.amazonaws.com/teowaki/users/3352/avatars/small.jpg?1"
  },
  "taglines": {
    "href": "https://loyal.guru/api/people/ada/taglines"
  },
  "new_tagline": {
    "href": "https://loyal.guru/api/people/ada/taglines/new"
  }
}
{% endhighlight %}

## Embedded Resources

Sometimes you want to get a resource and also its related resources in a single request. You can use the **api_embed** for that when using Loyal Guru API.

When a response contains **embedded** resources they will be an _embedded object. The object will have one entry per each of the embedded relations

## Lists as embedded resources

Every list of resources is represented as an embedded resource. 

Several results will be contained into a **_embedded** object, under the property **list**.

{% highlight json %}
"_embedded": {
"list": [
  {
    "created_at": "2013-12-18T01:46:58.458Z",
    "guid": 8268,
    "url": "http://www.allthingsdistributed.com/2007/10/amazons_dynamo.html",
    "uri": "/teams/javier-community/links/8268-amazon-s-dynamo-all-things-distributed",
    "description": "In two weeks we’ll present a p",
    "name": "Amazon's Dynamo - All Things Distributed",
    "scope": "Links::TeamLink"
  },
  {
    "created_at": "2014-03-31T18:39:51.801Z",
    "guid": 17759,
    "url": "http://en.wikipedia.org/wiki/Gazetteer",
    "uri": "/teams/javier-javier/links/17759-gazetteer-wikipedia-the-free-encyclopedia",
    "description": "A gazetteer is a geographicat",
    "name": "Gazetteer - Wikipedia, the free encyclopedia",
    "scope": "Links::TeamLink"
  }]
}
{% endhighlight %}

## Pagination

You can see the <code>_links</code> object contains link relations for paginating the results.

{% highlight json %}
"_links": {
  "prev": {
  "href": "https://loyal.guru/api/search?q=redis&amp;list_since=1"
  },
  "next": {
  "href": "https://loyal.guru/api/search?q=redis&amp;list_to=3"
  },
  "start": {
  "href": "https://loyal.guru/api/search?q=redis"
  }
}
{% endhighlight %}

If we are talking about the first page, no **prev** param will exist.

*** 

**Pendiente**

- New resources: What about calling a new resource endpoint in order to get a full object to avoid constructing manually the new forms.