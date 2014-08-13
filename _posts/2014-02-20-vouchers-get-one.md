---
category: Endpoints
path: '/vouchers/:id'
title: 'Vouchers get one'
type: 'GET'

layout: nil
---

This method allows to get certain voucher

### Request parameters

* `type` ( coupon, promotion, loyalty_score )
* `pending` If **true** the voucher is yet to be redeemed, if **false** the voucher is used.

### Response format

Check for the **discount** field in order to apply this discount to the purchase.

Check the standard status.