---
category: Endpoints ( Customers )
path: '/vouchers/:id'
title: 'Vouchers get one'
type: 'GET'

layout: nil
---

This method allows to get certain voucher.
Note that the id os the voucher is a HASH ID.

### Request parameters

* `type` ( coupon, promotion, loyalty_score )
* `pending` If **true** the voucher is yet to be redeemed, if **false** the voucher is used.

### Response format

Check for the **discount** field in order to apply this discount to the purchase.


### Response sample

Note: Redeem voucher url, voucheable object ( promotion related to voucher ) , and discount field.

```
{
  "_links": {
    "redeem": {
      "href": "http://localhost:3000/api/customers/me/vouchers/2/redeem"
    },
    "self": {
      "href": "http://localhost:3000/api/vouchers/2"
    }
  },
  "voucheable": {
    "_links": {
      "self": {
        "href": "http://localhost:3000/api/promotions/1"
      }
    },
    "location": {
      "_links": {
        "self": {
          "href": "http://localhost:3000/api/locations/1"
        }
      },
      "longitude": 2.2002152,
      "latitude": 41.4427448,
      "city": "Barcelona",
      "name": "La Maquinista",
      "id": 1
    },
    "discount": "20%",
    "type": "Coupon",
    "description": "xxxxx",
    "name": "20% decuento",
    "id": 1
  },
  "expiration_to": "2014-12-31",
  "expiration_from": "2014-01-01",
  "pending": true,
  "id": "8326747"
}
```



Check the standard status.