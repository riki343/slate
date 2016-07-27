---
title: FOODIZ API Reference

language_tabs:
  - general

toc_footers:

includes:
  - errors

search: true
---

# Introduction

Welcome to the FOODIZ API Documentation

# Authentication

## (REST) Authenticate via social

> Request:

```json
{
    "token": "747812726177660928-EPiElKQDIZRQyzB8mwxWP4JSccWRJIy",
    "social": "twitter",
    "secret": "t8BhG4EPkGdrJCiQ1BYh4Qu6Bq3ANMT3n3Ieevw9BgE68",
    "deviceType": "android",
    "deviceId": "8d3133382b007cf8",
    "pushToken": "fsL4lswpYjg:APA91bH5hhTP0JaOAGwpqQysukPrG1I4G1gmvKgmc38ORuhczOOcswkD7Dp43mh4mT7lfhpP4hp6doXCNHT3niwUt15PXJHQiJ_tVS_ChO81nfQGbTu_dH9wUq1qBvwaxA-dnoZkOR2E"
}
```

> Success Response:

```json
{
  "Authorization": "Bearer a-cmS6Sg41XaO6NzUQRvhlgi4Dt6np1469632525",
  "expiresAt": "2016-08-10 15:15:25",
  "userName": "Illia Hapak",
  "avatarUrl": "http://foodizfront.dev/files/images/gprDR_B2dZ_1468578009.jpg"
}
```

> Error Response

```json
{
  "name": "Bad Request",
  "message": "Missing required parameters: social",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

This endpoint authenticate user and retrieve token for application.

### HTTP Request

`POST http://52.50.150.232/api/login`
 <aside class="notice">
    Content-type available: application/json and form-data
 </aside>

### Request Parameters

Parameter   | Place | Type      | Description
----------- | ----- | --------- | -----------
token       | body  | string    | token from social
secret      | body  | string    | this parameter required only when social = "twitter"
deviceId    | body  | string    | unique device ID
social      | body  | string    | can be: "twitter", "facebook", "google"
deviceType  | body  | string    | "android", "desktop"(, "ios"?)
pushToken   | body  | string    | token for google cloud messaging

<aside class="success">
 Remember — all parameters is required, except secret and pushToken.
</aside>

### Success Response model

Parameter | Type     | Description
--------- | -------- | -----------
token     | string   | token for mobile app
expiresAt | string   | token expiration date (format: ISO 8601)


## (REST) Logout

> Success Response:

```json
{
    "message": "You logged out"
}
```

> 401 Error: token not found

> Error Response

```json
{
  "name": "Bad Request",
  "message": "Missing required parameters: social",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

This endpoint authenticate user and retrieve token for application.

### HTTP Request

`POST http://52.50.150.232/api/logout`

### Success Response model

Parameter | Type     | Description
--------- | -------- | -----------
message   | string   | 


# Chat

## (REST) Get Chats in area

> Request 

```json
{ 
	"addressLines": ["Dukhnovycha Street, 13", "Uzhhorod", "Zakarpats'ka oblast", "Ukraine"],
	"feature": "13",
	"admin": "Zakarpatska",
	"subAdmin": "null",
	"locality": "Uzhhorod",
	"thoroughfare": "Dukhnovycha Street",
	"postalCode": "null",
	"countryCode": "UA",
	"countryName": "Ukraine",
	"hasLatitude": true,
	"latitude": 48.623874,
	"hasLongitude": true,
	"longitude": 22.301063,
	"phone": "null",
	"url": "null",
	"extras": "null"
}
```

> Success Response:

```json
[
  {
    "id": 1,
    "title": "Uzhgorod best chat room",
    "country": "Ukraine",
    "city": "Uzhhorod"
  },
  {
    "id": 2,
    "title": "Kiev best chat room",
    "country": "Ukraine",
    "city": "Kiev"
  }
]
```

> Error Response

```json
{
  "name": "Bad Request",
  "message": "Missing required parameters: longitude",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

Getting all general chats in your country sorted by distances to your area.

### HTTP Request

`POST http://52.50.150.232/api/chats-around`

### Request Parameters

Parameter   | Place | Type        | Description
----------- | ----- | ----------  | -----------
address     | body  | json_object | Address from geocoder

<aside class="success">
 Remember — required fields of address only latitude and longitude.
</aside>

### Success Response model

Parameter   | Type     | Description
----------- | -------- | -----------
id          | integer  | id of chat room
title       | string   | chat title
country     | string   | chat country
city        | string   | chat city

# Event