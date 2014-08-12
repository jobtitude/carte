# API Resources

Loyal Guru exposes many resources via its API. Not all of the resources respond to <code>GET</code>, <code>POST</code>, <code>PUT</code> and <code>DELETE</code>. 

## First level resources 
### For Customers and Users
- sessions
- registrations
- passwords
- customers _( nested: redemptions, vouchers, activities )_
- rewards
- promotions
- locations

### For Users
- activities
- redemptions
- vouchers
- users

## Curl samples

```
curl -d '{"param": value, "param":value}' -H "Content-type: application/json" base_api_url
curl -d '{"param": value, "param":value}' -H "Content-type: application/json" -u xxx@xxx.com:token base_api_url
```

## Endpoint reference ( pending )
Base URL: 
- **production**: ```https://loyal.guru/api```
- **staging**: ```https://loyalty-jobtitude-staging.herokuapp.com/api```

| Method        | Endpoint                                               | Request                          |  AUTH         | USAGE |
|:--------------|--------------------------------------------------------|---------------------------------|--------------|--------:|
| GET           | /public                                             |      | |  HAL Root
| POST          | /customers                                             |     email, password, password_confirmation, company_id  | |  New register
| POST          | /customers/sign_in                                     |     email, password, company_id  |   | Signin
| DELETE        | /customers/sign_out                                    |     email, password, company_id  |   X | Signout
| POST          | /customers/password                                    |     email, company_id            |   X| Reset password
| PUT           | /customers                                             |      email, password, password_confirmation, current_password| X | Change password  
| GET           | /customers/{enter_field&#124;me}                       |      with_html    | X | Get user profile 
| GET           | /locations                                             |         near, distance, location{latitude, longitude},  | X | Get a list of locations
| GET           | /locations/{id}                                        |          | X | Get location info 
| GET           | /promotions                                            |         | X | Get a list of special promotions
| GET           | /promotions/{id}                                       |         | X | Get a special promotion detail
| GET           | /rewards                                               |         location_id  | X | Get a list of locations
| GET           | /rewards/{id}                                          |          | X | Get reward info 
| GET           | /customer/{enter_field&#124;me}/vouchers               |      type ( coupon, promotion, loyalty_score ), pending   | X | Vouchers from certains customer
| GET           | /voucher/{id}                                          |          | X | Get voucher info 
| GET           | /customer/{enter_field&#124;me}/redemptions            |         | X | Get customer redemptions
| POST          | /customer/{enter_field&#124;me}/redemptions/{reward_id}|         | X | Redeem certain reward by user
| GET           | /customer/{enter_field&#124;me}/activities             |        location_id, order_by | X | Gest activities from user
| GET           | /customer/{enter_field&#124;me}/rewards                |         | X | Available rewards for user



**Suitable for auth USERS ( Not CUSTOMERS )**

| Method        | Endpoint                          | Request                          |  USAGE 
| --------------|-----------------------------------| ---------------------------------|---------:|
| GET           | /users/{id or me}     |  | Get a user profile from your company
| POST          | /activities/                      |  customer_id ( opt ), total, location_id, lines_attributes                                |  Create activity
| GET           | /redemptions     |  | Get redemptions
| GET           | /vouchers     | same as customer/x/vouchers | Get vouchers
| POST          | /customers/{enter_field}/vouchers/redeem      | voucher_id | Redeem a voucher for certain customer


## /me

- You can also GET information from the currently authenticated user by calling <code>https://loyal.guru/api/users/me</code> or <code>https://loyal.guru/api/customers/me</code> 
- You can use **me** insted of user.id/custoer.membership_number when calling resources

## Nesting resources
- Get guidance from https://developers.teowaki.com/api-resources ( customers/id/rewards/, customers/
- Keep an eye on actions and states ( prefer: customers/1/rewards/active, customers/1/rewards/3/redeem, ... )

## Consistent paths formats 

### Resources names
Use the plural version of a resource name unless the resource in question is a singleton within the system (for example, in most systems a given user would only ever have one account). This keeps it consistent in the way you refer to particular resources.

### Downcase paths and attributes

Use downcased and dash-separated path names, for alignment with hostnames, e.g:
```
service-api.com/users
service-api.com/app-setups
````

Downcase attributes as well, but use underscore separators so that attribute names can be typed without quotes in JavaScript, e.g.:
```
"service_class": "first"
```

***
**Pending:**
- add PATCH calls to the API?
- add default values for optional params
- add error on unsupported methods: If you try to issue a verb which is not supported for the resource you will get a <code>404</code> not found status. ( May be with grape we got it? )