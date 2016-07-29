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

Key         | Place | Type      | Description
----------- | ----- | --------- | -----------
token       | body  | string    | token from social
secret      | body  | string    | this parameter required only when social = "twitter"
deviceId    | body  | string    | unique device ID
social      | body  | string    | can be: "twitter", "facebook", "google"
deviceType  | body  | string    | "android", "desktop"(, "ios"?)
pushToken   | body  | string    | token for google cloud messaging

<aside class="notice">
 Remember — all parameters is required, except secret and pushToken.
</aside>

### Success Response model

Key       | Type     | Description
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

Key       | Type     | Description
--------- | -------- | -----------
message   | string   | 


# Chat

## (REST) Get Chats in area

> Request:

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

Gets all general chats in your country, sorted on distance to the area indicated in the address.

### HTTP Request

`POST http://52.50.150.232/api/chats-around`

### Request Parameters

Key          | Place | Type        | Description
------------ | ----- | ----------- | -----------
addressLines | body  | json_array  | components of address
feature      | body  | string      | number on street
admin        | body  | string      | area
subAdmin     | body  | string      | region
locality     | body  | string      | city
thoroughfare | body  | string      | street
postalCode   | body  | string      | postal code of city
countryCode  | body  | string      | short country code
countryName  | body  | string      | name of country
hasLatitude  | body  | bool        | define that latitude is present
latitude     | body  | float       | latitude of place
hasLongitude | body  | bool        | define that longitude is present
longitude    | body  | float       | longitude of place
phone        | body  | string      | phone of place
url          | body  | string      | url of place
extra        | body  | string      | some extra field

<aside class="notice">
 Remember — required parameters of address only latitude and longitude.
</aside>

### Success Response

 Returns json array of [General Chat Room](#general-chat-room) models.


# Event section

## (REST) Change event status

> Request:

```json
{
    "id":6,
    "status":"canceled"
}
```

> Success Response

```json
{
  "success": true
}
```

> Error Response

```json
{
  "success": false,
  "error": "Status is invalid."
}
```

Change status of event.   
Status "canceled" - cancel event and notify all participants about cancel.

### HTTP Request
 
 `POST http://52.50.150.232:4567/api/event/change-status`
 
### Request Parameters

Key         | Place | Type        | Description
----------- | ----- | ----------  | -----------
id          | body  | integer     | id of event
status      | body  | string      | new status of event, can be: "active", "canceled", "finished"

<aside class="notice">
 Remember all parameters required
</aside>

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
success   | string   | It signifies successful completion of the request

## (REST) Create event

> Request:

```json
{
    "title":"Uzhgorod first event", 
    "date":"2016-08-07 16:44:00",
    "places":8,
    "description":"Test description",
    "address":"Ukraine, Uzhgorod, Dukhnovycha Street, 13",
    "coverUrl":"http://52.50.150.232/files/images/HtvaQ4JJz9_1468666614.jpg",
    "takeWithYou":"Wine, Juice",
    "latitude":22.4342,
    "longitude":49.432432
}
```

> Success Response

```json
{
  "id": 5,
  "title": "Uzhgorod first event",
  "userId": 1,
  "created": "2016-07-28 08:39:13",
  "updated": "2016-07-28 08:39:13",
  "date": "2016-08-07 16:44:00",
  "status": "active",
  "coverUrl": "http://52.50.150.232/files/images/HtvaQ4JJz9_1468666614.jpg",
  "places": 8,
  "description": "Test description",
  "address": "Ukraine, Uzhgorod, Dukhnovycha Street, 13",
  "takeWithYou": "Wine, Juice",
  "latitude": 22.4342,
  "longitude": 49.432432
}
```

> Error Response

```json
{
  "success": false,
  "error": "Cover not found!"
}
```

Creates a new event and returns the new event model.

### HTTP Request

`POST http://52.50.150.232/api/event/create`

### Request Parameters

Key         | Place | Type        | Description
----------- | ----- | ----------  | -----------
title       | body  | string      | title for new event
date        | body  | string      | date of the event (format: ISO 8601)
places      | body  | integer     | max count of people on event
description | body  | string      | short description about the event
address     | body  | string      | address place of event
coverUrl    | body  | string      | url of cover of event that resides on server
takeWithYou | body  | string      | count of things which participants should bring on event
latitude    | body  | float       | latitude place of event
longitude   | body  | float       | longitude place of event

<aside class="notice">
 Remember required only: title, date, coverUrl, places, latitude, longitude.
</aside>

### Success Response 

 Returns [Event](#event) model.

## (REST) Get event page

> Success Response

```json
{
  "event": {
    "id": 5,
    "title": "Uzhgorod first event",
    "userId": 1,
    "created": "2016-07-28 08:39:13",
    "updated": "2016-07-28 08:39:13",
    "date": "2016-08-07 16:44:00",
    "status": "active",
    "coverUrl": "http://52.50.150.232/files/images/HtvaQ4JJz9_1468666614.jpg",
    "places": 8,
    "description": "Test description",
    "address": "Ukraine, Uzhgorod, Dukhnovycha Street, 13",
    "takeWithYou": "Wine, Juice",
    "latitude": 22.4342,
    "longitude": 49.4324
  },
  "creator": {
    "id": 1,
    "username": "Illia Hapak",
    "google": null,
    "twitter": "747812726177660928",
    "facebook": null,
    "avatarUrl": "http://52.50.150.232/files/images/wDlvX-04N2_1468663003.jpg",
    "created": "2016-07-16 09:56:43",
    "updated": "2016-07-16 09:56:43",
    "blocked_till": null,
    "blocked_reason": null,
    "online": 0,
    "active": 1
  },
  "participants": [
    {
      "id": 2,
      "username": "Eduard Chory",
      "google": null,
      "twitter": "4311451583",
      "facebook": null,
      "avatarUrl": "http://52.50.150.232/files/images/tyaRLtgA7f_1468829754.png",
      "created": "2016-07-18 08:15:54",
      "updated": "2016-07-18 08:15:54",
      "blocked_till": null,
      "blocked_reason": null,
      "online": 0,
      "active": 1
    }
  ]
}
```

> Error Response

```json
{
  "success": false,
  "error": "Event not exist."
}
```

Returns json object which contain event, creator and participants.

### HTTP Request
 
 `GET http://52.50.150.232/api/get-event-page`
 
### Request Parameters

Key         | Place        | Type       | Description
----------- | -----        | ---------- | -----------
id          | query string | integer    | id of event

### Success Response model

Key          | Type        | Description
------------ | ----------- | -----------
event        | json object | [Event](#event) model
creator      | json object | [User](#user) model
participants | json array  | [User](#user) models

## (REST) Remove participant

> Success Response

```json
{
  "success": true
}
```

> Error Response

```json
{
  "success": false,
  "error": "User or event not exist"
}
```

Removing participant of event by HOST

### HTTP Request
 
 `GET http://52.50.150.232:4567/api/remove-participant`
 
### Request Parameters

Key         | Place        | Type    | Description
----------- | ------------ | ------- | -----------
eventId     | query string | integer | id of event
userId      | query string | integer | id of user

<aside class="notice">
 Remember only HOST of event can remove participants.
</aside>

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
success   | string   | It signifies successful completion of the request

## (REST) Repeat event

> Success Response

```json
{
  "id": 7,
  "title": "Uzhgorod event (updated)",
  "userId": 1,
  "created": "2016-07-28 14:24:27",
  "updated": "2016-07-28 14:24:27",
  "date": "2016-08-08 14:00:00",
  "status": "active",
  "coverUrl": "http://52.50.150.232/files/images/HtvaQ4JJz9_1468666614.jpg",
  "places": 8,
  "description": "Test description",
  "address": "Ukraine, Uzhgorod, Dukhnovycha Street, 13",
  "takeWithYou": "Wine, Juice",
  "latitude": 22.4342,
  "longitude": 49.4324
}
```

> Error Response

```json
{
  "success": false,
  "error": "Event not exist or not finished"
}
```

Repeats previously completed event.

### HTTP Request
 
 `GET http://52.50.150.232:4567/api/repeat-event`
 
### Request Parameters

Key         | Place | Type        | Description
----------- | ----- | ----------  | -----------
eventId     | query string | integer | id of event
date        | query string | string  | new date of the event (format: ISO 8601)

<aside class="notice">
 Remember when the event repeated it creates a new one event, event chat room and participants.
</aside>

### Success Response

 Returns [Event](#event) model.

## (REST) Update event

> Request:

```json
{
    "id":3,
    "title":"Test title updated" 
}
```

> Success Response

```json
{
  "id": 3,
  "title": "Test title updated",
  "userId": 1,
  "created": "2016-07-20 08:55:46",
  "updated": "2016-07-28 10:05:29",
  "date": "2016-08-07 16:44:00",
  "status": "active",
  "coverUrl": "http://foodizfront.dev/files/images/E7_Ths48l7_1468581507.jpg",
  "places": 8,
  "description": "Test description. Updated",
  "address": "Ukraine, Uzh",
  "takeWithYou": "2 bottles of wine",
  "latitude": 22,
  "longitude": 49
}
```

> Error Response

```json
{
  "success": false,
  "error": "Event ID is invalid."
}
```

Update an existing event model.

### HTTP Request

`POST http://52.50.150.232/api/event/update`

### Request Parameters

Key         | Place | Type        | Description
----------- | ----- | ----------  | -----------
id          | body  | integer     | id of event
title       | body  | string      | title for new event
date        | body  | string      | date of the event (format: ISO 8601)
places      | body  | integer     | max count of people on event
description | body  | string      | short description about the event
address     | body  | string      | address place of event
coverUrl    | body  | string      | url of cover of event that resides on server
takeWithYou | body  | string      | count of things which participants should bring on event
latitude    | body  | float       | latitude place of event
longitude   | body  | float       | longitude place of event

<aside class="notice">
 Remember required only id.
</aside>

### Success Response

 Returns [Event](#event) model.

# Models

## Event

> JSON:

```json
{
  "id": 3,
  "title": "Test title updated",
  "userId": 1,
  "created": "2016-07-20 08:55:46",
  "updated": "2016-07-28 10:05:29",
  "date": "2016-08-07 16:44:00",
  "status": "active",
  "coverUrl": "http://foodizfront.dev/files/images/E7_Ths48l7_1468581507.jpg",
  "places": 8,
  "description": "Test description. Updated",
  "address": "Ukraine, Uzh",
  "takeWithYou": "2 bottles of wine",
  "latitude": 22,
  "longitude": 49
}
```

Key         | Type     | Description
----------- | -------- | -----------
id          | integer  | id of event
title       | string   | title of event
userId      | integer  | id of creator of event
created     | string   | date of creation (format: ISO 8601)
updated     | string   | date of last modification (format: ISO 8601)
date        | string   | date of event (format: ISO 8601)
status      | string   | status of event
coverUrl    | string   | url of cover of event that resides on server
places      | integer  | max count of people on event
description | string   | short description about the event
address     | string   | address place of event
takeWithYou | string   | count of things which participants should bring on event
latitude    | float    | latitude place of event
longitude   | float    | longitude place of event

## General Chat Room

> JSON:

```json
{
    "id": 2,
    "title": "Kiev best chat room",
    "country": "Ukraine",
    "city": "Kiev"
  }
```

Key         | Type     | Description
----------- | -------- | -----------
id          | integer  | id of chat room
title       | string   | chat title
country     | string   | chat country
city        | string   | chat city

## Push Notification

> JSON:

```json
{
    "id": 1,
    "message": "By the event joined by Illia Hapak",
    "action": "Join to event.",
    "created": "2016-07-20 14:27:42"
  }
```

Key         | Type     | Description
----------- | -------- | -----------
id          | integer  | id of push notification
message     | string   | notification message
action      | string   | action when notification was created
created     | string   | date of creation (format: ISO 8601)


## User

> JSON:

```json
{
      "id": 2,
      "username": "Eduard Chory",
      "google": null,
      "twitter": "4311451583",
      "facebook": null,
      "avatarUrl": "http://52.50.150.232/files/images/tyaRLtgA7f_1468829754.png",
      "created": "2016-07-18 08:15:54",
      "updated": "2016-07-18 08:15:54",
      "blocked_till": null,
      "blocked_reason": null,
      "online": 0,
      "active": 1
    }
```

Key            | Type     | Description
-------------- | -------- | -----------
id             | integer  | id of user
username       | string   | username taken from social
google         | string   | google id
twitter        | string   | twitter id
facebook       | string   | facebook id
avatarUrl      | string   | url of avatar that resides on server
created        | string   | date of creation (format: ISO 8601)
updated        | string   | date of last modification (format: ISO 8601)
blocked_till   | string   | date of end block (format: ISO 8601)
blocked_reason | string   | Explain why user was blocked
 


# User settings and profile

## (REST) Deactivate account

> Success Response

```json
{
  "success": true
}
```

> Error Response

```json
{
  "name": "Unauthorized",
  "message": "You are requesting with an invalid credential.",
  "code": 0,
  "status": 401,
  "type": "yii\\web\\UnauthorizedHttpException"
}
```

Deactivate user account.

### HTTP Request
 
 `GET http://52.50.150.232:4567/api/deactivate-account`

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
success   | string   | It signifies successful completion of the request

## (REST) Disconnect social

> Success Response

```json
{
  "success": true
}
```

> Error Response

```json
{
  "success": false,
  "error": "Invalid social network"
}
```

Disconnect social network from user account.

### HTTP Request
 
 `GET http://52.50.150.232:4567/api/disconnect-social`
 
### Request Parameters

Key         | Place        | Type    | Description
----------- | ------------ | ------- | -----------
social      | query string | string  | can be: "twitter", "facebook", "google"

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
success   | string   | It signifies successful completion of the request

## (REST) Join to event

> Success Response

```json
{
  "success": true
}
```

> Error Response

```json
{
  "success": false,
  "error": "Event not exist."
}
```

Joining user to event.

### HTTP Request
 
 `GET http://52.50.150.232:4567/api/join-to-event`
 
### Request Parameters

Key         | Place        | Type    | Description
----------- | ------------ | ------- | -----------
id          | query string | integer | id of event

<aside class="notice">
 User will join to event if will free place for him.
</aside>

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
success   | string   | It signifies successful completion of the request

## (REST) Leave event

> Success Response

```json
{
  "success": true
}
```

> Error Response

```json
{
  "success": false,
  "error": "The user does not take part in the event."
}
```

Remove user from participants of event.

### HTTP Request
 
 `GET http://52.50.150.232:4567/api/leave-event`
 
### Request Parameters

Key         | Place        | Type    | Description
----------- | ------------ | ------- | -----------
id          | query string | integer | id of event

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
success   | string   | It signifies successful completion of the request

## (REST) View archive events

> Success Response

```json
[
  {
    "id": 4,
    "title": "Uzhgorod first event",
    "userId": 1,
    "created": "2016-07-26 11:45:07",
    "updated": "2016-07-26 11:45:07",
    "date": "2016-08-07 16:44:00",
    "status": "finished",
    "coverUrl": "http://52.50.150.232/files/images/HtvaQ4JJz9_1468666614.jpg",
    "places": 8,
    "description": "Test description",
    "address": "Ukraine, Uzhgorod, Dukhnovycha Street, 13",
    "takeWithYou": "Wine, Juice",
    "latitude": 22.4342,
    "longitude": 49.4324
  },
  {
    "id": 5,
    "title": "Uzhgorod first event",
    "userId": 1,
    "created": "2016-07-28 08:39:13",
    "updated": "2016-07-28 08:39:13",
    "date": "2016-08-07 16:44:00",
    "status": "canceled",
    "coverUrl": "http://52.50.150.232/files/images/HtvaQ4JJz9_1468666614.jpg",
    "places": 8,
    "description": "Test description",
    "address": "Ukraine, Uzhgorod, Dukhnovycha Street, 13",
    "takeWithYou": "Wine, Juice",
    "latitude": 22.4342,
    "longitude": 49.4324
  }
]
```

> Error Response

```json
{
  "name": "Unauthorized",
  "message": "You are requesting with an invalid credential.",
  "code": 0,
  "status": 401,
  "type": "yii\\web\\UnauthorizedHttpException"
}
```

Returns all finished or canceled events created by user.

### HTTP Request
 
 `GET http://52.50.150.232:4567/api/view-archive-events`

### Success Response 

 Returns json array of [Event](#event) models.

## (REST) View notifications list

> Success Response

```json
[
  {
    "id": 1,
    "message": "By the event joined by Illia Hapak",
    "action": "Join to event.",
    "created": "2016-07-20 14:27:42"
  },
  {
    "id": 2,
    "message": "User Illia Hapak canceled reservation.",
    "action": "Leave event.",
    "created": "2016-07-20 14:31:08"
  }
]
```

> Error Response

```json
{
  "name": "Unauthorized",
  "message": "You are requesting with an invalid credential.",
  "code": 0,
  "status": 401,
  "type": "yii\\web\\UnauthorizedHttpException"
}
```

Returns all push notifications.

### HTTP Request
 
 `GET http://52.50.150.232:4567/api/view-notifications-list`

### Success Response 

 Returns json array of [Push Notification](#push-notification) models.

## (REST) View own events

> Success Response

```json
[
  {
    "id": 4,
    "title": "Uzhgorod first event",
    "userId": 1,
    "created": "2016-07-26 11:45:07",
    "updated": "2016-07-26 11:45:07",
    "date": "2016-08-07 16:44:00",
    "status": "active",
    "coverUrl": "http://52.50.150.232/files/images/HtvaQ4JJz9_1468666614.jpg",
    "places": 8,
    "description": "Test description",
    "address": "Ukraine, Uzhgorod, Dukhnovycha Street, 13",
    "takeWithYou": "Wine, Juice",
    "latitude": 22.4342,
    "longitude": 49.4324
  },
  {
    "id": 5,
    "title": "Uzhgorod first event",
    "userId": 1,
    "created": "2016-07-28 08:39:13",
    "updated": "2016-07-28 08:39:13",
    "date": "2016-08-07 16:44:00",
    "status": "active",
    "coverUrl": "http://52.50.150.232/files/images/HtvaQ4JJz9_1468666614.jpg",
    "places": 8,
    "description": "Test description",
    "address": "Ukraine, Uzhgorod, Dukhnovycha Street, 13",
    "takeWithYou": "Wine, Juice",
    "latitude": 22.4342,
    "longitude": 49.4324
  }
]
```

> Error Response

```json
{
  "name": "Unauthorized",
  "message": "You are requesting with an invalid credential.",
  "code": 0,
  "status": 401,
  "type": "yii\\web\\UnauthorizedHttpException"
}
```

Returns all active events created by user.

### HTTP Request
 
 `GET http://52.50.150.232:4567/api/view-own-events`

### Success Response 

 Returns json array of [Event](#event) models.
 
## (REST) View upcoming events

> Success Response

```json
[
  {
    "id": 4,
    "title": "Uzhgorod first event",
    "userId": 1,
    "created": "2016-07-26 11:45:07",
    "updated": "2016-07-26 11:45:07",
    "date": "2016-08-07 16:44:00",
    "status": "active",
    "coverUrl": "http://52.50.150.232/files/images/HtvaQ4JJz9_1468666614.jpg",
    "places": 8,
    "description": "Test description",
    "address": "Ukraine, Uzhgorod, Dukhnovycha Street, 13",
    "takeWithYou": "Wine, Juice",
    "latitude": 22.4342,
    "longitude": 49.4324
  },
  {
    "id": 5,
    "title": "Uzhgorod first event",
    "userId": 1,
    "created": "2016-07-28 08:39:13",
    "updated": "2016-07-28 08:39:13",
    "date": "2016-08-07 16:44:00",
    "status": "active",
    "coverUrl": "http://52.50.150.232/files/images/HtvaQ4JJz9_1468666614.jpg",
    "places": 8,
    "description": "Test description",
    "address": "Ukraine, Uzhgorod, Dukhnovycha Street, 13",
    "takeWithYou": "Wine, Juice",
    "latitude": 22.4342,
    "longitude": 49.4324
  }
]
```

> Error Response

```json
{
  "name": "Unauthorized",
  "message": "You are requesting with an invalid credential.",
  "code": 0,
  "status": 401,
  "type": "yii\\web\\UnauthorizedHttpException"
}
```

Returns all active events in which the user is participant.

### HTTP Request
 
 `GET http://52.50.150.232:4567/api/view-upcoming-events`

### Success Response 

 Returns json array of [Event](#event) models.




