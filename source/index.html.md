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
        "type": "fixed",
        "vehicleType": "scooter",
        "unattendedDeliveryOption": "safeplace",
        "pickupTime": "2016-05-03T12:31:00.000Z",
        "latestPickupTime": "2016-05-03T15:31:00.000Z",
        "pickupAddress": "1 Primrose St, London EC2A 2EX, UK",
        "dropoffAddress": "1 Commercial St, London E1 6LP, UK",
        "merchantId": "xxx",
        "pickupLocationId": "xxx",
        "dropoffLocationId": "xxx",
        "customerFee": 7.6,
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
        "reference": "194055",
        "driverName": "Ayman",
        "driverId": "1234124542342",
        "lastUpdated": "2016-05-03T12:50:09.990Z",
        "created": "2016-05-03T12:14:06.297Z",
        "receiver": {
            "name": "Arthur",
            "phone": "12345645646",
            "email": "ms@email.com"
        },
        "sender": {
            "name": "Mr John Doe",
            "phone": "07xxxxxxxx",
            "email": "mrx@email.com"
        },
        "dropoffTime": {
            "latestTime": "2016-05-03T12:50:27.000Z",
            "earliestTime": "2016-05-03T12:50:27.000Z"
        },
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
    "type": "fixed",
    "vehicleType": "scooter",
    "unattendedDeliveryOption": "alt",
    "pickupTime": "2016-05-03T12:31:00.000Z",
    "latestPickupTime": "2016-05-03T15:31:00.000Z",
    "pickupAddress": "1 Primrose St, London EC2A 2EX, UK",
    "dropoffAddress": "1 Commercial St, London E1 6LP, UK",
    "merchantId": "xxx",
    "pickupLocationId": "xxx",
    "dropoffLocationId": "xxx",
    "customerFee": 7.6,
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
    "reference": "194055",
    "driverName": "Ayman",
    "driverId": "56b346a8f3e7800a28812bbb",
    "lastUpdated": "2016-05-03T12:50:09.990Z",
    "created": "2016-05-03T12:14:06.297Z",
    "receiver": {
        "name": "Arthur",
        "phone": "12345645646",
        "email": "ms@email.com"
    },
    "sender": {
        "name": "Mr John Doe",
        "phone": "07xxxxxxxx",
        "email": "mrx@email.com"
    },
    "dropoffTime": {
        "latestTime": "2016-05-03T12:50:27.000Z",
        "earliestTime": "2016-05-03T12:50:27.000Z"
    },
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
    "type": "open",
    "vehicleType": "van",
    "earliestPickupTime": "2016-02-22T11:55:37.847008+00:00",
    "latestPickupTime": "2016-02-22T16:55:37.847008+00:00",  
    "pickupLocation": {
        "name": "CatMan",
        "phone": "0740xxxxxx",
        "email": "mr@catface.com",
        "streetNumber": "1",
        "street": "Primrose Street",
        "postCode": "EC2A2EX",
        "city": "London"
    },
    "dropoffLocation": {
        "name": "DogMan",
        "phone": "0745xxxxxx",
        "email": "sir@dogface.com",
        "streetNumber": "1",
        "street": "Commercial Street",
        "postCode": "E1 6LP",
        "city": "London"
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
    "earliestPickupTime": "2016-05-10T11:55:37.847Z",
    "latestPickupTime": "2016-05-10T16:55:37.847Z",
    "merchantId": "571618db84a6f0241fd7e4f6",
    "pickupLocationId": "56b1da6ef3e7800a288129c2",
    "dropoffLocationId": "56b1dddef3e7800a288129c8",
    "customerFee": 11.5,
    "quote": {
        "deliveryTime": "2016-05-10T12:46:34.847Z",
        "distance": "6.23",
        "minutes": "51",
        "distUnit": "mile",
        "currency": "GBP",
        "cost": "11.50",
        "_id": "57305643929641291f76cb4e"
    },
    "currentStatus": "Booked",
    "_id": "57305643929641291f76cb4d",
    "lastUpdated": "2016-05-09T09:20:03.056Z",
    "created": "2016-05-09T09:20:03.056Z",
    "receiver": {
        "name": "DogMan",
        "phone": "0745xxxxxx",
        "email": "sir@dogface.com"
    },
    "sender": {
        "name": "CatMan",
        "phone": "0740xxxxxx",
        "email": "mr@catface.com"
    },
    "dropoffTime": {
        "earliestTime": "2016-05-10T12:46:34.847Z",
        "latestTime": "2016-05-10T12:46:34.847Z"
    },
    "items": []
}
```

This endpoint adds a new delivery order into the system.


### HTTP Request

`POST https://api.swishd.com/v1/deliveries`



### Book Delivery Fields

Parameter | Required | Description
--------- | ------- | -----------
reference | no | If this is not included, Swishd will auto-generate a reference number
type | **yes** | **open** or **fixed** - Fixed Deliveries must use the ​*pickupTime*​ parameter. Open Deliveries must use the *earliestPickupTime & ​*latestPickupTime*​ parameters.
vehicleType | **yes** | **van** or **scooter** - Fixed Deliveries can use ​*scooter*​ parameter. Open Deliveries can use the ​*scooter*​ or ​*van*​ parameter.
pickupTime | **yes** | Fixed Deliveries must use the ​*pickupTime*​ parameter.
earliestPickupTime | **yes** | Required for Open Deliveries. Used in conjunction with the *latestPickupTime*​ parameter. Use the ​*earliestPickupTime*​ parameter to signify the start of a window. eg. Delivery between ​*earliestPickupTime*​ 09:00 and ​*latestPickupTime*​ 12:00.
latestPickupTime | **yes** | Required for Open Deliveries. Use the ​*latestPickupTime*​ parameter to signify the end of a window. eg. Delivery between ​*earliestPickupTime*​ 09:00 and ​*latestPickupTime*​ 12:00.
pickupLocation | **yes** | Object including the parameters of the Pickup item
name | **yes** | Name at Pickup Address (Person/Business Name)
phone | **yes** | Phone No. of Pickup Address
email | **yes** | Email address of Pickup Address
streetNumber | **yes** | Street Number
street | **yes** | Street Name
postCode | **yes** | PostCode
city | **yes** | City
dropoffLocation | **yes** | Object including the parameters of the Dropoff item
name | **yes** | Name at Dropoff Address (Person/Business Name)
phone | **yes** | Phone No. of Dropoff Address
email | **yes** | Email address of Dropoff Address
streetNumber | **yes** | Street Number
street | **yes** | Street Name
postCode | **yes** | PostCode
city | **yes** | City
deliveryInstructions | no | Any additional information that is required to be passed to the driver
unattendedDeliveryOption | **yes** | Options include: **safeplace** (Leave in a safe place) , **neighbour** (Leave item with Neighbour), **alt** (Delivery to Alternative Address - Will incur additional cost), **returnsender** (Delivery to Alternative Address - Will incur additional cost)
unattendedDeliveryNote | no | Notes relating to the *unattendedDeliveryOption*
items: [] | No | Optional field where a **collection** of items can be passed to ensure the delivery driver picks up the correct number of items. Parameters in this collection include *Quantity* (integer) and ​*Description*​ (string).
requestedAddress | No | If the pickup/dropoff address are not formatted in the correct manner, we can attempt to geocode it by passing this in the ​*pickupLocation*​ or ​*dropoffLocation*​ object. We highly suggest using this as a Quote first to ensure the Address returned is the correct one.


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
    "type": "open",
    "vehicleType": "van",
    "earliestPickupTime": "2016-02-22T11:55:37.847008+00:00",
    "latestPickupTime": "2016-02-22T16:55:37.847008+00:00",  
    "pickupLocation": {
        "name": "CatMan",
        "phone": "0740xxxxxx",
        "email": "mr@catface.com",
        "streetNumber": "1",
        "street": "Primrose Street",
        "postCode": "EC2A2EX",
        "city": "London"
    },
    "dropoffLocation": {
        "name": "DogMan",
        "phone": "0745xxxxxx",
        "email": "sir@dogface.com",
        "streetNumber": "1",
        "street": "Commercial Street",
        "postCode": "E1 6LP",
        "city": "London"
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
    "earliestPickupTime": "2016-05-10T11:55:37.847Z",
    "latestPickupTime": "2016-05-10T16:55:37.847Z",
    "receiver": {
        "name": "DogMan",
        "phone": "0745xxxxxx",
        "email": "sir@dogface.com"
    },
    "sender": {
        "name": "CatMan",
        "phone": "0740xxxxxx",
        "email": "mr@catface.com"
    },
    "quoteOnly": true,
    "customerFee": "11.50",
    "dropoffTime": {
        "earliestTime": "2016-05-10T12:46:34.847Z",
        "latestTime": "2016-05-10T12:46:34.847Z"
    },
    "quote": {
        "origin": "1 Primrose St, London EC2A 2EX, UK",
        "destination": "1 Commercial St, London E1 6LP, UK",
        "deliveryTime": "2016-05-10T12:46:34.847Z",
        "distance": "6.23",
        "minutes": "51",
        "distUnit": "mile",
        "currency": "GBP",
        "cost": "11.50"
    }
}
```

This endpoint returns a quote. It essentially mimics the new delivery endpoint without registering the delivery in the system and
returning an estimated price and other delivery information.


### HTTP Request

`POST https://api.swishd.com/v1/quote`


### Quote Delivery Fields

Parameter | Required | Description
--------- | ------- | -----------
reference | no | If this is not included, Swishd will auto-generate a reference number
type | **yes** | **open** or **fixed** - Fixed Deliveries must use the ​*pickupTime*​ parameter. Open Deliveries must use the *earliestPickupTime & ​*latestPickupTime*​ parameters.
vehicleType | **yes** | **van** or **scooter** - Fixed Deliveries can use ​*scooter*​ parameter. Open Deliveries can use the ​*scooter*​ or ​*van*​ parameter.
pickupTime | **yes** | Fixed Deliveries must use the ​*pickupTime*​ parameter.
earliestPickupTime | **yes** | Required for Open Deliveries. Used in conjunction with the *latestPickupTime*​ parameter. Use the ​*earliestPickupTime*​ parameter to signify the start of a window. eg. Delivery between ​*earliestPickupTime*​ 09:00 and ​*latestPickupTime*​ 12:00.
latestPickupTime | **yes** | Required for Open Deliveries. Use the ​*latestPickupTime*​ parameter to signify the end of a window. eg. Delivery between ​*earliestPickupTime*​ 09:00 and ​*latestPickupTime*​ 12:00.
pickupLocation | **yes** | Object including the parameters of the Pickup item
name | **no** | Name at Pickup Address (Person/Business Name)
phone | **no** | Phone No. of Pickup Address
email | **no** | Email address of Pickup Address
streetNumber | **yes** | Street Number
street | **yes** | Street Name
postCode | **yes** | PostCode
city | **yes** | City
dropoffLocation | **yes** | Object including the parameters of the Dropoff item
name | **no** | Name at Dropoff Address (Person/Business Name)
phone | **no** | Phone No. of Dropoff Address
email | **no** | Email address of Dropoff Address
streetNumber | **yes** | Street Number
street | **yes** | Street Name
postCode | **yes** | PostCode
city | **yes** | City
deliveryInstructions | no | Any additional information that is required to be passed to the driver
unattendedDeliveryOption | **yes** | Options include: **safeplace** (Leave in a safe place) , **neighbour** (Leave item with Neighbour), **alt** (Delivery to Alternative Address - Will incur additional cost), **returnsender** (Delivery to Alternative Address - Will incur additional cost)
unattendedDeliveryNote | no | Notes relating to the *unattendedDeliveryOption*
items: [] | No | Optional field where a **collection** of items can be passed to ensure the delivery driver picks up the correct number of items. Parameters in this collection include *Quantity* (integer) and *Description* (string).
requestedAddress | No | If the pickup/dropoff address are not formatted in the correct manner, we can attempt to geocode it by passing this in the *pickupLocation* or *dropoffLocation* object. We highly suggest using this as a Quote first to ensure the Address returned is the correct one.
