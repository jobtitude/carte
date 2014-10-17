---
category: Endpoints ( Users )
path: '/activities'
title: 'Activities create'
type: 'POST'

layout: nil
---

This method allows to create an activity.

### Request parameters

* `customer_id` (optional) If customer_id not corresponds to any customer the activity will be saved anyway with the id served. 
* `total`
* `location_id` (optional) If location_id not corresponds to any location the activity will be saved anyway with the id served. If none locatin_id is existent the system will use the user location session ( not the customer, but the user inserting the activity )
* `location_code` (optional) That is the id that the company uses to identify his location.
* `lines_attributes{product_id, order,quantity, total}`


Sample:
{% highlight bash %}
  curl -v http://loyalty-jobtitude-staging.herokuapp.com/api/activities 
  -u prueba@jobtitude.com:user-2HZDS1rAKn9dZdzFV4HA 
  -H "Content-Type:application/json" 
  -H "Accept: application/vnd.loyalguru.v1"  
  -d '{
    "customer_id":"1", 
    "code":"200",
    "total":"4000", 
    "location_code":"123",
    "voucher_code":"xxxxxxx", 
    "lines_attributes": [
        {"product_code":"XR7654","quantity":"3","order":"1","price":"50","discount_type":"20%", "discount":"200.99", "size":"L", "color":"yellow", "custom_1":"WIN14"},
        {"product_code":"45678","quantity":"4","order":"2","price":"199"}
        ]
  }'
{% endhighlight %}

### Response format

Check the standard status.