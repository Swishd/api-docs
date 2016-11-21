---
title: Swishd Public API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='https://app.swishd.com'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

This is the Swishd public delivery API documentation. It allows you to automate delivery requests that Swishd delivery network will fulfil.
In order to use the API, you need to be an authorised Swishd merchant. To do this, you need to register at [app.swishd.com](https://app.swishd.com).
Then you can obtain an api key from the merchant profile section.

# Authentication

> To authorize, use this code:


```shell
# With shell, you can just pass the correct header with each request
curl -H "Authorization: Bearer [the key]"
    https://api.swishd.com

```

Swishd uses API keys to allow access to the API. You can obtain a new API key at [app.swishd.com](https://app.swishd.com).

Swishd expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer [the key]`



# Deliveries

## Get All My Deliveries

```shell  
curl -H "Authorization: Bearer [the key]"
    "https://api.swishd.com/v1/deliveries?page=1&pageSize=10"
```

> The above command returns JSON structured like this:

```json
[
    {
        "_id": "[order id]",
        "type": "[service type]",
        "vehicleType": "scooter",
        "unattendedDeliveryOption": "safeplace",        
        "pickup": {
          "name": "Mr John Doe",
          "email": "mrx@email.com",
          "earliestPickupTime": "[pickup can start at that time. ISO format]",
          "pickupTime": "[estimated actual pickup time]",
          "latestPickupTime": "[pikup should happen before that time]",
          "address": "1 Primrose St, London EC2A 2EX, UK",
          "customerId": "xxx"
        },
        "dropoff": {
          "name": "Arthur",
          "email": "ms@email.com",
          "address": "1 Commercial St, London E1 6LP, UK",
          "dropoffTime": "[estimated drop-off time]",
          "latestDropoffTime": "[latest allowed time for drop-off]",
          "customerId": "xxx"
        },
        "quote": {
            "deliveryTime": "[ETA for drop-off]",
            "distance": "1.87",
            "minutes": "19",
            "distUnit": "mile",
            "currency": "GBP",
            "cost": "7.60"
        },
        "currentStatus": "Completed",
        "jobId": "[internal use]",
        "merchantId": "[internal use]",
        "reference": "[customer ref]",
        "driverName": "Ayman",
        "driverId": "[internal use]",
        "lastUpdated": "2016-05-03T12:50:09.990Z",
        "created": "2016-05-03T12:14:06.297Z",
        "items": []
    }
]
```

This endpoint retrieves all deliveries belonging to your business.

### HTTP Request

`GET https://api.swishd.com/v1/deliveries`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
page | yes | Page number
pageSize | yes | Number of elements per page

<aside class="success">
page and pageSize are mandatory to prevent fetching a huge dataset.
</aside>

## Get a Specific Delivery


```shell
curl -H "Authorization: Bearer [the key]"
    "https://api.swishd.com/v1/deliveries/id"
```

> The above command returns JSON structured like this:

```json
{
    "_id": "[order id]",
    "type": "[service type]",
    "vehicleType": "scooter",
    "unattendedDeliveryOption": "alt",    
    "pickup": {
      "name": "Mr John Doe",
      "phone": "07xxxxxxxx",
      "email": "mrx@email.com",
      "earliestPickupTime": "[pickup can start at that time. ISO format]",
      "pickupTime": "[estimated actual pickup time]",
      "latestPickupTime": "[pikup should happen before that time]",
      "address": "1 Primrose St, London EC2A 2EX, UK",
      "geoCodedAddress": "1 Primrose St, London EC2A 2EX, UK",
      "customerId": "xxx",
      "description": "xxx",
      "companyName": ""
    },
    "dropoff": {
      "name": "Arthur",
      "phone": "12345645646",
      "email": "ms@email.com",
      "address": "1 Commercial St, London E1 6LP, UK",
      "geoCodedAddress": "1 Commercial St, London E1 6LP, UK",
      "dropoffTime": "[estimated drop-off time]",
      "latestDropoffTime": "[latest allowed time for drop-off]",
      "customerId": "xxx",
      "description": "xxx",
      "companyName": ""
    },
    "quote": {
        "deliveryTime": "[ETA]",
        "distance": "1.87",
        "minutes": "19",
        "distUnit": "mile",
        "currency": "GBP",
        "cost": "7.60",
        "_id": "5728961744c8c8241f2f70d9"
    },
    "currentStatus": "Completed",
    "jobId": "xxx",
    "merchantId": "xxx",
    "reference": "194055",
    "driverName": "Ayman",
    "driverId": "56b346a8f3e7800a28812bbb",
    "lastUpdated": "2016-05-03T12:50:09.990Z",
    "created": "2016-05-03T12:14:06.297Z",    
    "items": []
}
```

This endpoint retrieves a specific delivery.

<aside class="warning">You need to know the delivery id</aside>

### HTTP Request

`GET https://api.swishd.com/v1/deliveries/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the delivery to retreive


## Book a Delivery


```shell
curl -H "Authorization: Bearer [the key]" -X "POST" --data @body.json
    "https://api.swishd.com/v1/deliveries"
```

> The document to send:

```json
{
    "reference": "TEST01",
    "deliveryInstructions": "Handle with care",
    "vehicleType": "[van|scooter]",    
    "pickup": {
        "name": "CatMan",
        "companyName": "CatMan Ltd.",
        "phone": "0740xxxxxx",
        "email": "mr@catface.com",
        "streetNumber": "1",
        "street": "Primrose Street",
        "postCode": "EC2A2EX",
        "city": "London",
        "earliestPickupTime": "[pickup can start at that time. ISO format]",
        "pickupTime": "[deprecated]",
        "latestPickupTime": "[deprecated]"
    },
    "dropoff": {
        "name": "DogMan",        
        "phone": "0745xxxxxx",
        "email": "sir@dogface.com",
        "streetNumber": "1",
        "street": "Commercial Street",
        "postCode": "E1 6LP",
        "city": "London",
        "description": "",
        "latestDropoffTime": "[latest allowed time for drop-off, ISO format]"
    }
}
```

> The above command returns JSON structured like this:

```json
{
    "type": "open",
    "vehicleType": "van",
    "reference": "TEST01",
    "deliveryInstructions": "Handle with care",
    "merchantId": "571618db84a6f0241fd7e4f6",
    "quote": {
        "deliveryTime": "[ETA for drop off]",
        "distance": "6.23",
        "minutes": "51",
        "distUnit": "mile",
        "currency": "GBP",
        "cost": "11.50"
    },
    "currentStatus": "Booked",
    "_id": "57305643929641291f76cb4d",
    "lastUpdated": "2016-05-09T09:20:03.056Z",
    "created": "2016-05-09T09:20:03.056Z",
    "dropoff": {
        "dropoffTime": "[estimated dropoff time]",
        "latestDropoffTime": "[The originally requested latest drop off time]",
        "name": "DogMan",
        "companyName": "",
        "phone": "0745xxxxxx",
        "email": "sir@dogface.com",
        "customerId": "..."
    },
    "pickup": {        
        "earliestPickupTime": "[the requested earliest pickup time]",
        "pickupTime": "[Estimated pickup time]",
        "latestPickupTime": "[Estimated latest pickup time to arrive before latest drop-off]",
        "name": "CatMan",
        "companyName": "CatMan Ltd.",        
        "phone": "0740xxxxxx",
        "email": "mr@catface.com",
        "customerId": "56b1dddef3e7800a288129c8"
    },
    "items": []
}
```

This endpoint adds a new delivery order into the system.


### HTTP Request

`POST https://api.swishd.com/v1/deliveries`



### Book Delivery Fields

Parameter | Required | Description
--------- | -------- | -----------
reference | no | If this is not included, Swishd will auto-generate a reference number
vehicleType | **yes** | **van** or **scooter**. Van deliveries need a pickup - dropoff window of at least 3 hours.
pickup | **yes** | Object including the parameters of the Pickup item
pickup.earliestPickupTime | **yes** | Delivery (pickup) can start from that moment.
pickup.name | **yes** | Name at Pickup Address (Person Name)
pickup.companyName | **yes** | Name at Pickup Address (Business Name)
pickup.phone | **yes** | Phone No. of Pickup Address
pickup.email | **yes** | Email address of Pickup Address
pickup.streetNumber | **yes** | Street Number
pickup.street | **yes** | Street Name
pickup.postCode | **yes** | PostCode
pickup.city | **yes** | City
pickup.address | No | If the pickup/dropoff address are not formatted in the correct manner, we can attempt to geocode it by passing this in the ​*pickup*​ or ​*dropoff*​ object. We highly suggest using this as a Quote first to ensure the Address returned is the correct one.
dropoff | **yes** | Object including the parameters of the Dropoff item
dropoff.name | **yes** | Name at Dropoff Address (Person Name)
dropoff.companyName | **yes** | Name at Dropoff Address (Business Name)
dropoff.phone | **yes** | Phone No. of Dropoff Address
dropoff.email | **yes** | Email address of Dropoff Address
dropoff.streetNumber | **yes** | Street Number
dropoff.street | **yes** | Street Name
dropoff.postCode | **yes** | PostCode
dropoff.city | **yes** | City
dropoff.address | No | If the pickup/dropoff address are not formatted in the correct manner, we can attempt to geocode it by passing this in the ​*pickup*​ or ​*dropoff*​ object. We highly suggest using this as a Quote first to ensure the Address returned is the correct one.
dropoff.latestDropoffTime | No | The parcel needs to be delivered before this time. Price and vehicle types can depend on the urgency of the delivery.
deliveryInstructions | no | Any additional information that is required to be passed to the driver
unattendedDeliveryOption | **yes** | Options include: **safeplace** (Leave in a safe place) , **neighbour** (Leave item with Neighbour), **alt** (Delivery to Alternative Address - Will incur additional cost), **returnsender** (Delivery to Alternative Address - Will incur additional cost)
unattendedDeliveryNote | no | Notes relating to the *unattendedDeliveryOption*
items: [] | No | Optional field where a **collection** of items can be passed to ensure the delivery driver picks up the correct number of items.


Item structure:
```json
{
   "quantity": "Number",
   "sku": "[SKU code, String]",
   "description": "String",
   "price": "Number"
}
```


## Cancel delivery


```shell
curl -X "POST" --data @body.json
    "https://api.swishd.com/v1/deliveries/cancel/id"
```
> The above command returns JSON structured like this:

```json
{
    "_id": "[order id]",
    "type": "[service type]",
    "vehicleType": "scooter",
    "unattendedDeliveryOption": "alt",    
    "dropoff": {
        "dropoffTime": "[estimated dropoff time]",
        "latestDropoffTime": "[The originally requested latest drop off time]",
        "name": "DogMan",
        "companyName": "",
        "phone": "0745xxxxxx",
        "email": "sir@dogface.com",
        "customerId": "..."
    },
    "pickup": {        
        "earliestPickupTime": "[the requested earliest pickup time]",
        "pickupTime": "[Estimated pickup time]",
        "latestPickupTime": "[Estimated latest pickup time to arrive before latest drop-off]",
        "name": "CatMan",
        "companyName": "CatMan Ltd.",        
        "phone": "0740xxxxxx",
        "email": "mr@catface.com",
        "customerId": "56b1dddef3e7800a288129c8"
    },
    "quote": {
        "deliveryTime": "2016-05-03T12:50:27.000Z",
        "distance": "1.87",
        "minutes": "19",
        "distUnit": "mile",
        "currency": "GBP",
        "cost": "7.60",
        "_id": "5728961744c8c8241f2f70d9"
    },
    "currentStatus": "Completed",
    "jobId": "xxx",
    "merchantId": "xxx",
    "reference": "194055",
    "driverName": "Ayman",
    "driverId": "56b346a8f3e7800a28812bbb",
    "lastUpdated": "2016-05-03T12:50:09.990Z",
    "created": "2016-05-03T12:14:06.297Z",    
    "items": []
}
```
This endpoint cancels a specific delivery.

<aside class="warning">You need to know the delivery id</aside>

### HTTP Request

`GET https://api.swishd.com/v1/deliveries/cancel/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the delivery to cancel




## Get a Quote


```shell
curl -X "POST" --data @body.json
    "https://api.swishd.com/v1/quote"
```
> The document to send:

```json
{
    "reference": "TEST01",
    "deliveryInstructions": "Handle with care",
    "vehicleType": "van",    
    "pickup": {
        "name": "CatMan",
        "phone": "0740xxxxxx",
        "email": "mr@catface.com",
        "streetNumber": "1",
        "street": "Primrose Street",
        "postCode": "EC2A2EX",
        "city": "London",
        "earliestPickupTime": "[pickup can start at that time. ISO format]",
        "pickupTime": "[deprecated]",
        "latestPickupTime": "[deprecated]"
    },
    "dropoff": {
        "name": "DogMan",
        "phone": "0745xxxxxx",
        "email": "sir@dogface.com",
        "streetNumber": "1",
        "street": "Commercial Street",
        "postCode": "E1 6LP",
        "city": "London",
        "latestDropoffTime": "[latest allowed time for drop-off, ISO format, optional]"
    }
}
```

> The above command returns JSON structured like this:

```json
{
    "origin": "1 Primrose St, London EC2A 2EX, UK",
    "destination": "1 Commercial St, London E1 6LP, UK",
    "deliveryTime": "2016-05-10T12:46:34.847Z",
    "distance": "6.23",
    "minutes": "51",
    "distUnit": "mile",
    "currency": "GBP",
    "cost": "11.50"
}
```

This endpoint returns a quote with an estimated price, distance and delivery time.


### HTTP Request

`POST https://api.swishd.com/v1/quote`


### Quote Delivery Fields

Parameter | Required | Description
--------- | ------- | -----------
reference | no | If this is not included, Swishd will auto-generate a reference number
vehicleType | **yes** | **van** or **scooter** - Fixed Deliveries can use ​*scooter*​ parameter. Open Deliveries can use the ​*scooter*​ or ​*van*​ parameter.
pickup | **yes** | Object including the parameters of the Pickup item
pickup.name | **no** | Name at Pickup Address (Person/Business Name)
pickup.phone | **no** | Phone No. of Pickup Address
pickup.email | **no** | Email address of Pickup Address
pickup.streetNumber | **yes** | Street Number
pickup.street | **yes** | Street Name
pickup.postCode | **yes** | PostCode
pickup.city | **yes** | City
pickup.address | No | If the pickup/dropoff address are not formatted in the correct manner, we can attempt to geocode it by passing this in the *pickup* or *dropoff* object. We highly suggest using this as a Quote first to ensure the Address returned is the correct one.
pickup.earliestPickupTime | **yes** | Delivery (pickup) can start from that moment.
dropoff | **yes** | Object including the parameters of the Dropoff item
dropoff.name | **no** | Name at Dropoff Address (Person/Business Name)
dropoff.phone | **no** | Phone No. of Dropoff Address
dropoff.email | **no** | Email address of Dropoff Address
dropoff.streetNumber | **yes** | Street Number
dropoff.street | **yes** | Street Name
dropoff.postCode | **yes** | PostCode
dropoff.city | **yes** | City
dropoff.address | No | If the pickup/dropoff address are not formatted in the correct manner, we can attempt to geocode it by passing this in the *pickup* or *dropoff* object. We highly suggest using this as a Quote first to ensure the Address returned is the correct one.
dropoff.latestDropoffTime | No | The parcel needs to be delivered before this time. Price and vehicle types can depend on the urgency of the delivery.
