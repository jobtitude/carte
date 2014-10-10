# API Resources

Loyal Guru exposes many resources via its API. Not all of the resources respond to **GET**, **POST**, **PUT** and **DELETE**. 

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

{% highlight bash %}
curl -d '{"param": value, "param":value}' -H "Content-type: application/json" base_api_url
curl -d '{"param": value, "param":value}' -H "Content-type: application/json" -u xxx@xxx.com:token base_api_url
{% endhighlight %}

## /me

- You can also GET information from the currently authenticated user by calling <code>https://loyal.guru/api/users/me</code> or <code>https://loyal.guru/api/customers/me</code> 
- You can use **me** insted of user.id/custoer.membership_number when calling resources

## Nesting resources

- Get guidance from https://developers.teowaki.com/api-resources ( customers/id/rewards/, customers/
- Keep an eye on actions and states ( prefer: customers/1/rewards/active, customers/1/rewards/3/redeem, ... )

## Consistent paths formats 

### Resources names
Use the plural version of a resource name unless the resource in question is a singleton within the system (for example, in most systems a given user would only ever have one account). This keeps it consistent in the way you refer to particular resources.

### Resources attributes

**To code or to ID**:   
You'l se often references to **code** and **id**. **id** is the internal id of our Loyal Guru systema, while **code** is the Id the comapny may have assigned to certain object in their database.
For example a certain product **code** for a company may be XRZ784JEANS while our **id** will be 143566.

You can use in certain api calls one of the two, but keep in mind that if your send a **code** that is not beofre created and assigned in the loyal guru backend, a new item will be created with the code.

### Downcase paths and attributes

Use downcased and dash-separated path names, for alignment with hostnames, e.g:

```
service-api.com/users
service-api.com/app-setups
```

Downcase attributes as well, but use underscore separators so that attribute names can be typed without quotes in JavaScript, e.g.:

```
"service_class": "first"
```

***

**Pending:**

- add PATCH calls to the API?
- add default values for optional params
- add error on unsupported methods: If you try to issue a verb which is not supported for the resource you will get a <code>404</code> not found status. ( May be with grape we got it? )