---
category: Endpoints
path: '/locations'
title: 'Locations listing'
type: 'GET'

layout: nil
---

This method allows to get a list of locations. You can specify a **near** parameter in order to get locations near a specific **location** in a radius of **distance** meters.

### Request parameters

* `near`
* `distance`
* `location{latitude, longitude}`

### Response format

Check the standard status.