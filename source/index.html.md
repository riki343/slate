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

> Success Response:

```json
{
    "token": "ddadu6dwatdfawd8awfae8w9awf12bjklwhfblkvjlanwerkcj",
    "expiresAt": "2005-08-15T15:52:01+0000"
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

### Request Parameters

Parameter   | Place | Type      | Description
----------- | ----- | --------- | -----------
token       | body  | string    | token from social
secret      | body  | string    | this parameter required only when social = "twitter"
deviceId    | body  | string    | unique device ID
social      | body  | string    | can be: "twitter", "facebook", "google"
deviceType  | body  | string    | "android", "desktop"(, "ios"?)

<aside class="success">
 Remember — all parameters is required, except secret
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
    "addressLines":{
        "0":"Dukhnovycha Street, 13А",
        "1":"Uzhhorod",
        "2":"Zakarpats'ka oblast",
        "3":"Ukraine"
    },
    "feature":null,
    "admin":"Zakarpats'ka oblast",
    "sub-admin":null,
    "locality":"Uzhgorod",
    "thoroughfare":"Dukhnovycha Street",
    "postalCode":null,
    "countryCode":"UA",
    "countryName":"Ukraine",
    "hasLatitude":true,
    "latitude":48.623831,
    "hasLongitude":true,
    "longitude":22.301231,
    "phone":null,
    "url":null,
    "extras":null
}
```

> Success Response:

```json
[
  {
    "id": 1,
    "title": "Uzhgorod chatroom",
    "country": "Ukraine",
    "city": "Uzhgorod"
  },
  {
    "id": 3,
    "title": "Uzhgorod chatroom 2",
    "country": "Ukraine",
    "city": "Uzhgorod"
  }
]
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

Get all general chats in your area

### HTTP Request

`POST http://52.50.150.232/api/general-chat-room/chats-around`

### Request Parameters

Parameter   | Place | Type       | Description
----------- | ----- | ---------- | -----------
address     | body  | json_array | Address from geocoder

### Success Response model

Parameter   | Type     | Description
----------- | -------- | -----------
id          | integer  | id of chat room
title       | string   | chat title
country     | string   | chat country
city        | string   | chat city