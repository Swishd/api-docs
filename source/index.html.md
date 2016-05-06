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
        "_id": "5728961744c8c8241f2f70d8",
        "type": "fixed",
        "vehicleType": "scooter",
        "unattendedDeliveryOption": "4",
        "pickupTime": "2016-05-03T12:31:00.000Z",
        "latestPickupTime": "2016-05-03T15:31:00.000Z",
        "pickupAddress": "225, Central Markets, London EC1A 9LH, United Kingdom",
        "dropoffAddress": "40 Commercial St, London E1 6LP, United Kingdom",
        "merchantId": "571618db84a6f0241fd7e4f6",
        "pickupLocationId": "57152868f3e7800a2881498c",
        "dropoffLocationId": "57152869f3e7800a2881498d",
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
        "jobId": "a823fa99-2bfd-46e7-b466-54449a88738e",
        "reference": "194055",
        "driverName": "Jakir Hussain",
        "driverId": "56b346a8f3e7800a28812bbb",
        "trackingUrls": {
            "api": "https://app.getswift.co/api/v2/deliveries/a823fa99-2bfd-46e7-b466-54449a88738e",
            "www": "https://app.getswift.co/t/Be3ed"
        },
        "lastUpdated": "2016-05-03T12:50:09.990Z",
        "created": "2016-05-03T12:14:06.297Z",
        "receiver": {
            "name": "The Culpeper",
            "phone": "12345645646",
            "email": "arthur.bilalov@swishd.com"
        },
        "sender": {
            "name": "Icefront, 222",
            "phone": "07710 649826",
            "email": "rm@joinfoodchain.com"
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
    "_id": "5728961744c8c8241f2f70d8",
    "type": "fixed",
    "vehicleType": "scooter",
    "unattendedDeliveryOption": "4",
    "pickupTime": "2016-05-03T12:31:00.000Z",
    "latestPickupTime": "2016-05-03T15:31:00.000Z",
    "pickupAddress": "225, Central Markets, London EC1A 9LH, United Kingdom",
    "dropoffAddress": "40 Commercial St, London E1 6LP, United Kingdom",
    "merchantId": "571618db84a6f0241fd7e4f6",
    "pickupLocationId": "57152868f3e7800a2881498c",
    "dropoffLocationId": "57152869f3e7800a2881498d",
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
    "jobId": "a823fa99-2bfd-46e7-b466-54449a88738e",
    "reference": "194055",
    "driverName": "Jakir Hussain",
    "driverId": "56b346a8f3e7800a28812bbb",
    "trackingUrls": {
        "api": "https://app.getswift.co/api/v2/deliveries/a823fa99-2bfd-46e7-b466-54449a88738e",
        "www": "https://app.getswift.co/t/Be3ed"
    },
    "lastUpdated": "2016-05-03T12:50:09.990Z",
    "created": "2016-05-03T12:14:06.297Z",
    "receiver": {
        "name": "The Culpeper",
        "phone": "12345645646",
        "email": "arthur.bilalov@swishd.com"
    },
    "sender": {
        "name": "Icefront, 222",
        "phone": "07710 649826",
        "email": "rm@joinfoodchain.com"
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
curl -H "Authorization: Bearer [the key]" -X "POST"
    "https://api.swishd.com/v1/deliveries"
```

> The above command returns JSON structured like this:

```json

```

This endpoint adds a new delivery order into the system.


### HTTP Request

`POST https://api.swishd.com/v1/deliveries`


## Get a Quote


```shell
curl -H "Authorization: Bearer [the key]" -X "POST"
    "https://api.swishd.com/v1/quote"
```

> The above command returns JSON structured like this:

```json

```

This endpoint returns a quote. It essentially mimics the new delivery endpoint without registering the delivery in the system and
returning an estimated price and other delivery information.


### HTTP Request

`POST https://api.swishd.com/v1/quote`

