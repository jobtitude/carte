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
* `lines_attributes{product_id, order,quantity, total}`



### Response format

Check the standard status.