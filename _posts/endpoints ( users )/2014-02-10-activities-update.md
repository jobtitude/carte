---
category: Endpoints ( Users )
path: '/activities'
title: 'Activities update'
type: 'PUT'

layout: nil
---

This method allows to update an activity.

### Request parameters

* `code` The internal ID of the company activity 
* `Id` ( optional ) The id that Loyal Guru uses to identify this activity
* `customer_id` (optional) If customer_id not corresponds to any customer the activity will be saved anyway with the id served. 
* `total`
* `location_id` (optional) If location_id not corresponds to any location the activity will be saved anyway with the id served. If none locatin_id is existent the system will use the user location session ( not the customer, but the user inserting the activity )
* `location_code` (optional) That is the id that the company uses to identify his location.
* `lines_attributes{product_id, order,quantity, total}`
* **lines_attributes** es el array que contiene cada producto ( **product_code**: id del producto para la empresa, **product_id**: el id en el sistema de Loyal Guru, **quantity**: cantidad de productos en ticket, **order**: orden de la línea de ticket, **price**: precio del artículo ) 
* **voucher_code**: sería el código del cupón validado  



### Response format

Check the standard status.