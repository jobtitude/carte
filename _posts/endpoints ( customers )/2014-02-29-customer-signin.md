---
category: Endpoints ( Customers )
path: '/customers/sign_in'
title: 'Customer signin'
type: 'POST'

layout: nil
---

This method allows to get the **autentication token** in order to proceed with the rest of the calls.

### Request parameters

* `email`
* `password`
* `company_id`

### Response format

Look for the `token` on the response and check the standard status.