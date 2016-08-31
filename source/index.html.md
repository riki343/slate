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
  "avatarUrl": "http://foodizfront.dev/files/images/gprDR_B2dZ_1468578009.jpg",
  "id": 1
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

Key           | Type     | Description
------------- | -------- | -----------
Authorization | string   | token for mobile app
expiresAt     | string   | token expiration date (format: ISO 8601)
username      | string   | user name from social networks
avatarUrl     | string   | url of avatar that resides on server
id            | integer  | id of user


## (REST) Logout

> Success Response:

```json
{
    "success": true
}
```

> Error Response

```json
{
  "name": "Internal Server Error",
  "message": "Failed to logout the user for unknown reason!",
  "code": 0,
  "status": 500,
  "type": "yii\\web\\ServerErrorHttpException"
}
```

Destroys user access token.

### HTTP Request

`POST http://52.50.150.232/api/logout`

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
success   | bool     | It signifies successful completion of the request


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
 
## (REST)Last messages: event chat room

> Success Response: 

```json
[
  {
    "isOwner": false,
    "userId": 3,
    "userName": "Eduard Chory",
    "userAvatar": "http://52.50.150.232/files/images/wn5Iof8I0w_1468836563.png",
    "messageId": 9,
    "messageType": "message",
    "messageCreated": "2016-07-27 16:46:05",
    "message": "gtv"
  },
  {
    "isOwner": false,
    "userId": 3,
    "userName": "Eduard Chory",
    "userAvatar": "http://52.50.150.232/files/images/wn5Iof8I0w_1468836563.png",
    "messageId": 10,
    "messageType": "message",
    "messageCreated": "2016-07-27 16:48:21",
    "message": "ggy"
  }
]
```

 Loads last messages from event chat room.
 
### HTTP Request
 
 `GET http://52.50.150.232/api/message/event-last`
 
### Request Parameters

Key           | Place        | Type              | Description
------------- | ------------ | ----------------- | -----------
chatId        | query string | integer           | id of event chat room
count         | query string | integer(optional) | count of messages for load, 20 by default
lastMessageId | query string | integer(optional) | id from which messages will be loaded, null by default
type          | query string | string(optional)  | type of messages, null by default

### Success Response
 
 Returns array of [Event Chat Room Message](#event-chat-room-message) models.
 
## (REST)Last messages: general chat room

> Success Response: 

```json
[
  {
    "isOwner": false,
    "userId": 3,
    "userName": "Eduard Chory",
    "userAvatar": "http://52.50.150.232/files/images/wn5Iof8I0w_1468836563.png",
    "messageId": 9,
    "messageType": "message",
    "messageCreated": "2016-07-27 16:46:05",
    "message": "gtv"
  },
  {
    "isOwner": false,
    "userId": 3,
    "userName": "Eduard Chory",
    "userAvatar": "http://52.50.150.232/files/images/wn5Iof8I0w_1468836563.png",
    "messageId": 10,
    "messageType": "message",
    "messageCreated": "2016-07-27 16:48:21",
    "message": "ggy"
  }
]
```

 Loads last messages from general chat room.
 
### HTTP Request
 
 `GET http://52.50.150.232/api/message/general-last`
 
### Request Parameters

Key           | Place        | Type              | Description
------------- | ------------ | ----------------- | -----------
chatId        | query string | integer           | id of general chat room
count         | query string | integer(optional) | count of messages for load, 20 by default
lastMessageId | query string | integer(optional) | id from which messages will be loaded, null by default
type          | query string | string(optional)  | type of messages, null by default

### Success Response
 
 Returns array of [General Chat Room Message](#general-chat-room-message) models.

## (REST) Send report

> Request:

```json
{
    "messageType": "general_chat",
    "messageId": 4,
    "chatId": 1,
    "reportedType": "other",
    "messageAuthorId": 7,
    "reason": "Some reason"
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
  "name": "Bad Request",
  "message": "Missing required parameters: reason",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

Sends report to moderator.

### HTTP Request
 
 `POST http://52.50.150.232/api/send-report`
 
### Request Parameters

Key             | Place | Type        | Description
--------------- | ----- | ----------  | -----------
messageType     | body  | string      | define type of chat room, can be: "general_chat", "event_chat"
messageId       | body  | integer     | id of message
chatId          | body  | integer     | id of chat
reportedType    | body  | string      | type of report message, can be: "cheater", "aggressive", "other"
messageAuthorId | body  | integer     | id of author of message
reason          | body  | string      | description of report message

<aside class="notice">
 Remember all parameters required, except reason if reportedType not "other"
</aside>

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
success   | bool     | It signifies successful completion of the request
 
## (REST)Update event chat room logo

> Request:

```json
{ 
	"eventId": 1,
	"url": "http://52.50.150.232/files/images/pgV4RyqiCZ_1469803519.png"
}
```

> Success Response: 

```json
{
  "url": "http://52.50.150.232/files/images/pgV4RyqiCZ_1469803519.png"
}
```

> Error Response:

```json
{
  "name": "Bad Request",
  "message": "Invalid Url!",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

 Changes logo of event chat room.
 
### HTTP Request
 
 `GET http://52.50.150.232/api/event-room/update-logo`
 
 <aside class="notice">
      Content-type available: application/json and form-data.
 </aside>
 
### Request Parameters

Key     | Place   | Type     | Description
------- | ------- | -------- | -----------
eventId | body    | string   | id of event
url     | body    | string   | url of new logo that resides on server

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
url       | string   | url of image that resides on server

## (REST)Update event chat room title

> Request:

```json
{ 
	"eventId": 1,
	"title": "New title"
}
```

> Success Response: 

```json
{
  "success": true
}
```

> Error Response:

```json
{
  "name": "Bad Request",
  "message": "You are not owner of this Event!",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

 Changes title of event chat room.
 
### HTTP Request
 
 `GET http://52.50.150.232/api/event-room/update-title`
 
 <aside class="notice">
      Content-type available: application/json and form-data.
 </aside>
 
### Request Parameters

Key     | Place   | Type     | Description
------- | ------- | -------- | -----------
eventId | body    | string   | id of event
title   | body    | string   | new title for event chat room

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
success   | bool     | It signifies successful completion of the request

## (WS) Connect

> Request:

```json
{
    "action": "connect", 
    "token": "zGEUmcAVZbRj6dncTAv6uGnsyrgA_U1468771457", 
    "chatType": "general", 
    "chatId": "1"
}
```

> Success Response:

```json
{
    "success":true,
    "message":"You have been connected successfully."
}
```

> Error Response:

```json
{
    "success":false,
    "message":"Failed to access!"
}
```

 Connect to chat.
 
### Request Parameters

Key         | Place | Type        | Description
----------- | ----- | ----------  | -----------
action      | body  | string      | action on server side
token       | body  | string      | access token
chatType    | body  | string      | can be: "general", "event"
chatId      | body  | string      | id of chat

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
success   | bool     | It signifies successful completion of the request
message   | string   | message to client

## (WS) Send Message

> Request:

```json
{
    "action": "message", 
    "token": "zGEUmcAVZbRj6dncTAv6uGnsyrgA_U1468771457", 
    "chatType": "general", 
    "chatId": "1", 
    "messageType": "message", 
    "message": "hello!!", 
    "key": "1234567"
}
```

> Success Response:

```json
{
    "userId":"18",
    "userName":"Vitalik Lunyov",
    "userAvatar":"http:\/\/52.50.150.232\/?r=files%2Fimages%2FC7CDzSvDzv_1467885361.jpg",
    "messageId":"37",
    "messageType":"message",
    "messageCreated":"2016-07-26 13:52:15",
    "message":"hello!!",
    "key": "1234567"
}
```

 Send plain message into chat.
 
### Request Parameters

Key         | Place | Type        | Description
----------- | ----- | ----------  | -----------
action      | body  | string      | action on server side
token       | body  | string      | access token
chatType    | body  | string      | can be: "general", "event"
chatId      | body  | string      | id of chat
messageType | body  | string      | can be: "message", "photo", "video", "event"
message     | body  | string      | message content
key         | body  | string      | request id

### Success Response model

Key            | Type     | Description
-------------- | -------- | -----------
userId         | string   | id of user
userName       | string   | user name from social networks
userAvatar     | string   | url of avatar that resides on server
messageId      | string   | id of message
messageType    | string   | can be: "message", "photo", "video", "event"
messageCreated | string   | date of creation (format: ISO 8601)
message        | string   | message content
key            | string   | request id

## (WS) Send Photo

> Request:

```json
{
    "action": "message",
    "token": "zGEUmcAVZbRj6dncTAv6uGnsyrgA_U1468771457",
    "chatType": "general",
    "chatId": "1",
    "messageType": "photo",
    "url": "http://foodiz.local/files/images/lWrdCqYAgA_1467928754.jpg",
    "key": "1234567"
}
```

> Success Response:

```json
{
    "userId":"18",
    "userName":"Vitalik Lunyov",
    "userAvatar":"http:\/\/52.50.150.232\/?r=files%2Fimages%2FC7CDzSvDzv_1467885361.jpg",
    "messageId":"38",
    "messageType":"photo",
    "messageCreated":"2016-07-26 13:52:58",
    "url":"http:\/\/foodiz.local\/files\/images\/lWrdCqYAgA_1467928754.jpg",
    "key": "1234567"
}
```

 Send photo into chat.
 
### Request Parameters

Key         | Place | Type        | Description
----------- | ----- | ----------  | -----------
action      | body  | string      | action on server side
token       | body  | string      | access token
chatType    | body  | string      | can be: "general", "event"
chatId      | body  | string      | id of chat
messageType | body  | string      | can be: "message", "photo", "video", "event"
url         | body  | string      | url of photo
key         | body  | string      | request id

### Success Response model

Key            | Type     | Description
-------------- | -------- | -----------
userId         | string   | id of user
userName       | string   | user name from social networks
userAvatar     | string   | url of avatar that resides on server
messageId      | string   | id of message
messageType    | string   | can be: "message", "photo", "video", "event"
messageCreated | string   | date of creation (format: ISO 8601)
url            | string   | url of photo
key            | string   | request id

## (WS) Send Video

> Request:

```json
{
    "action": "message",
    "token": "kMMxjsbVtDTEbKIh5a8NW8AnK3cenx1467979106",
    "chatType": "general",
    "chatId": "1",
    "messageType": "video",
    "url": "https://www.youtube.com/watch?v=op3eNwtSGfM",
    "key": "1234567"
}
```

> Success Response:

```json
{
    "userId":"18",
    "userName":"Vitalik Lunyov",
    "userAvatar":"http:\/\/52.50.150.232\/?r=files%2Fimages%2FC7CDzSvDzv_1467885361.jpg",
    "messageId":"39",
    "messageType":"video",
    "messageCreated":"2016-07-26 13:55:07",
    "url":"https:\/\/www.youtube.com\/watch?v=op3eNwtSGfM",
    "key": "1234567"
}
```

 Send video into chat.
 
### Request Parameters

Key         | Place | Type        | Description
----------- | ----- | ----------  | -----------
action      | body  | string      | action on server side
token       | body  | string      | access token
chatType    | body  | string      | can be: "general", "event"
chatId      | body  | string      | id of chat
messageType | body  | string      | can be: "message", "photo", "video", "event"
url         | body  | string      | url of video
key         | body  | string      | request id

### Success Response model

Key            | Type     | Description
-------------- | -------- | -----------
userId         | string   | id of user
userName       | string   | user name from social networks
userAvatar     | string   | url of avatar that resides on server
messageId      | string   | id of message
messageType    | string   | can be: "message", "photo", "video", "event"
messageCreated | string   | date of creation (format: ISO 8601)
url            | string   | url of video
key            | string   | request id

## (WS) Send Event

> Request:

```json
{
    "action": "message",
    "token": "kMMxjsbVtDTEbKIh5a8NW8AnK3cenx1467979106",
    "chatType": "general",
    "chatId": "1",
    "messageType": "event",
    "eventId": "1",
    "key": "1234567"
}
```

> Success Response:

```json
{
    "userId":"18",
    "userName":"Vitalik Lunyov",
    "userAvatar":"http:\/\/52.50.150.232\/?r=files%2Fimages%2FC7CDzSvDzv_1467885361.jpg",
    "messageId":"43",
    "messageType":"event",
    "messageCreated":"2016-07-26 14:04:06",
    "event":{
        "id":"1",
        "title":"test",
        "userId":"18",
        "created":"2016-07-12 20:00:00",
        "updated":"2016-07-12 20:00:00",
        "date":"2016-07-15 20:00:00",
        "status":"active",
        "coverUrl":null,
        "places":"0",
        "description":"",
        "address":"",
        "takeWithYou":null,
        "latitude":0,
        "longitude":0
    }, 
    "key": "1234567"
}
```

> Error Response

```json
{
    "success":false,
    "message":"Failed to access!"
}
```

 Send video into chat.
 
### Request Parameters

Key         | Place | Type        | Description
----------- | ----- | ----------  | -----------
action      | body  | string      | action on server side
token       | body  | string      | access token
chatType    | body  | string      | can be: "general", "event"
chatId      | body  | string      | id of chat
messageType | body  | string      | can be: "message", "photo", "video", "event"
eventId     | body  | string      | id of event
key         | body  | string      | request id

### Success Response model

Key            | Type          | Description
-------------- | ------------- | -----------
userId         | string        | id of user
userName       | string        | user name from social networks
userAvatar     | string        | url of avatar that resides on server
messageId      | string        | id of message
messageType    | string        | can be: "message", "photo", "video", "event"
messageCreated | string        | date of creation (format: ISO 8601)
event          | json object   | [Event](#event) model
key            | string        | request id

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

Changes status of event.   
Status "canceled" - cancel event and notify all participants about cancel.

### HTTP Request
 
 `POST http://52.50.150.232/api/event/change-status`
 
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
success   | bool     | It signifies successful completion of the request

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
    "longitude":49.432432,
    "chatRoomId":1,
    "reservedPlaces": 0,
    "hostUsername": "Illia Hapak",
    "hostAvatar": "http://foodizfront.dev/files/images/gprDR_B2dZ_1468578009.jpg",
    "rating": 3.5
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
  "longitude": 49.432432,
  "chatRoomId":1
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
chatRoomId  | body  | integer     | id of general chat room

<aside class="notice">
 Remember required only: title, date, coverUrl, places, latitude, longitude.
</aside>

### Success Response 

 Returns [Event](#event) model.

## (REST) Created events in room

> Success Response

```json
{
  "count": 6
}
```

> Error Response

```json
{
  "name": "Bad Request",
  "message": "Invalid Chat ID!",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

Returns all active events in requested general chat room.

### HTTP Request
 
 `GET http://52.50.150.232/api/events-created-in-room`
 
### Request Parameters

Key         | Place        | Type       | Description
----------- | ------------ | ---------- | -----------
chatId      | query string | integer    | id of general chat room

### Success Response model

Key   | Type    | Description
------| --------| -----------
count | integer | count of active events in requested general chat room

## (REST) Get comments

> Success Response

```json
[
  {
    "id": 1,
    "message": "Test comment",
    "created": "2016-08-04 13:26:34",
    "username": "Illia Hapak",
    "userAvatar": "http://foodizfront.dev/files/images/gprDR_B2dZ_1468578009.jpg"
  },
  {
    "id": 2,
    "message": "Test comment",
    "created": "2016-08-04 13:27:34",
    "username": "Illia Hapak",
    "userAvatar": "http://foodizfront.dev/files/images/gprDR_B2dZ_1468578009.jpg"
  }
]
```

> Error Response

```json
{
  "name": "Bad Request",
  "message": "Invalid Event ID!",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

Returns all posted comments for selected event.

### HTTP Request
 
 `GET http://52.50.150.232/api/event-comments`
 
### Request Parameters

Key         | Place        | Type       | Description
----------- | -----        | ---------- | -----------
eventId     | query string | integer    | id of event

### Success Response

 Returns json array of [Comment](#comment) models.

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
    "longitude": 49.4324,
    "chatRoomId": 1,
    "reservedPlaces": 0,
    "hostUsername": "Illia Hapak",
    "hostAvatar": "http://foodizfront.dev/files/images/gprDR_B2dZ_1468578009.jpg",
    "rating": 3.5
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

## (REST) Post comment

> Request:

```json
{
    "eventId":18,
    "message":"Test comment"
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
  "name": "Bad Request",
  "message": "Message should contain at most 1,024 characters.",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

Posts a new comment about event. 

### HTTP Request
 
 `POST http://52.50.150.232/api/post-comment`
 
### Request Parameters

Key         | Place | Type    | Description
----------- | ----- | ------- | -----------
eventId     | body  | integer | id of event
message     | body  | string  | comment content

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
success   | bool     | It signifies successful completion of the request

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
 
 `GET http://52.50.150.232/api/remove-participant`
 
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
success   | bool     | It signifies successful completion of the request

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
  "longitude": 49.4324,
  "chatRoomId": 1,
  "reservedPlaces": 0,
  "hostUsername": "Illia Hapak",
  "hostAvatar": "http://foodizfront.dev/files/images/gprDR_B2dZ_1468578009.jpg",
  "rating": 3.5
}
```

> Error Response

```json
{
  "name": "Bad Request",
  "message": "Invalid Event ID!",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

Repeats previously completed event.

### HTTP Request
 
 `GET http://52.50.150.232/api/repeat-event`
 
### Request Parameters

Key         | Place        | Type    | Description
----------- | ------------ | ------- | -----------
eventId     | query string | integer | id of event
date        | query string | string  | new date of the event (format: ISO 8601)

<aside class="notice">
 Remember when the event repeated it creates a new one event, event chat room and participants.
</aside>

### Success Response

 Returns [Event](#event) model.

## (REST) Set event rating

> Success Response

```json
{
  "success": true,
  "rating": 3.5
}
```

> Error Response

```json
{
  "name": "Bad Request",
  "message": "Rating must be no greater than 5.",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

Adds new rating for event.

### HTTP Request
 
 `GET http://52.50.150.232/api/set-event-rating`
 
### Request Parameters

Key         | Place        | Type    | Description
----------- | ------------ | ------- | -----------
eventId     | query string | integer | id of event
rating      | query string | integer | new rating value

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
success   | bool     | It signifies successful completion of the request
rating    | float    | current event rating


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
  "longitude": 49,
  "chatRoomId": 1,
  "reservedPlaces": 0,
  "hostUsername": "Illia Hapak",
  "hostAvatar": "http://foodizfront.dev/files/images/gprDR_B2dZ_1468578009.jpg",
  "rating": 3.5
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

# Files

## (REST)Upload Image

> Success Response: 

```json
{
  "url": "http://52.50.150.232/files/images/pgV4RyqiCZ_1469803519.png"
}
```

Error Response:

```json
{
  "name": "Bad Request",
  "message": "File cannot be blank.",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

 Uploading image and returns url of image
 
### HTTP Request
 
 `GET http://52.50.150.232/api/upload/image`
 
### Request Parameters

Key     | Place   | Type     | Description
------- | ------- | -------- | -----------
file    | body    | file     | Allowable extensions: "jpeg", "jpg", "png"

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
url       | string   | url of image that resides on server

## (REST)Upload Video

> Success Response: 

```json
{
  "url": "https://www.youtube.com/watch?v=op3eNwtSGfM"
}
```

 Uploading video via url.
 
### HTTP Request
 
 `GET http://52.50.150.232/api/upload/video`
 
 <aside class="notice">
     Content-type available: application/json and form-data
 </aside>
 
### Request Parameters

Key     | Place   | Type     | Description
------- | ------- | -------- | -----------
url     | body    | string   | url of video resource
hash    | body    | string   | hash of video

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
url       | string   | url of video resource

# Models

## Comment

> JSON:

```json
{
    "id": 1,
    "message": "Test comment",
    "created": "2016-08-04 13:26:34",
    "username": "Illia Hapak",
    "userAvatar": "http://foodizfront.dev/files/images/gprDR_B2dZ_1468578009.jpg"
}
```

Key            | Type     | Description
-------------- | -------- | -----------
id             | integer  | id of comment
message        | string   | content of comment
created        | string   | date of creation (format: ISO 8601)
username       | string   | username of author
userAvatar     | string   | url of author avatar


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
  "longitude": 49,
  "chatRoomId": 1,
  "reservedPlaces": 0,
  "hostUsername": "Illia Hapak",
  "hostAvatar": "http://foodizfront.dev/files/images/gprDR_B2dZ_1468578009.jpg",
  "rating": 3.5
}
```

Key            | Type     | Description
-------------- | -------- | -----------
id             | integer  | id of event
title          | string   | title of event
userId         | integer  | id of creator of event
created        | string   | date of creation (format: ISO 8601)
updated        | string   | date of last modification (format: ISO 8601)
date           | string   | date of event (format: ISO 8601)
status         | string   | status of event
coverUrl       | string   | url of cover of event that resides on server
places         | integer  | max count of people on event
description    | string   | short description about the event
address        | string   | address place of event
takeWithYou    | string   | count of things which participants should bring on event
latitude       | float    | latitude place of event
longitude      | float    | longitude place of event
chatRoomId     | integer  | id of general chat room
reservedPlaces | integer  | count of participants
hostUsername   | string   | username of creator
hostAvatar     | string   | url of host avatar
rating         | float    | rating of event

## Event Chat Room Message

> JSON:

```json
{
    "isOwner": false,
    "userId": 3,
    "userName": "Eduard Chory",
    "userAvatar": "http://52.50.150.232/files/images/wn5Iof8I0w_1468836563.png",
    "messageId": 9,
    "messageType": "message",
    "messageCreated": "2016-07-27 16:46:05",
    "message": "gtv"
}
```

Key            | Type               | Description
-------------- | ------------------ | -----------
isOwner        | bool               | defines that current user is author of the message
userId         | integer            | id of user
userName       | string             | username taken from social
userAvatar     | string             | url of avatar that resides on server
messageId      | integer            | id of message
messageType    | string             | can be: "message", "photo", "video"
messageCreated | string             | date of creation
message        | string/json object | content of message

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

## General Chat Room Message

> JSON:

```json
{
    "isOwner": false,
    "userId": 3,
    "userName": "Eduard Chory",
    "userAvatar": "http://52.50.150.232/files/images/wn5Iof8I0w_1468836563.png",
    "messageId": 9,
    "messageType": "message",
    "messageCreated": "2016-07-27 16:46:05",
    "message": "gtv"
}
```

Key            | Type               | Description
-------------- | ------------------ | -----------
isOwner        | bool               | defines that current user is author of the message
userId         | integer            | id of user
userName       | string             | username taken from social
userAvatar     | string             | url of avatar that resides on server
messageId      | integer            | id of message
messageType    | string             | can be: "message", "photo", "video", "event", if type "event" then message will contain [Event](#event) model
messageCreated | string             | date of creation
message        | string/json object | content of message

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

## (REST)Refresh connection

> Success Response:

```json
{
    "success": true
}
```

> Error Response

```json
{
  "name": "Internal Server Error",
  "message": "Failed to connect for unknown reason!",
  "code": 0,
  "status": 500,
  "type": "yii\\web\\ServerErrorHttpException"
}
```

 Saves the current screen name in app to user statistic and adds 60 seconds total time spent in app. 

 <aside class="notice">
  This method should call every minute!
 </aside>
 
### HTTP Request
 
 `GET http://52.50.150.232/api/connect`
 
### Request Parameters

Key           | Place        | Type    | Description
------------- | ------------ | ------- | -----------
lastScreen    | query string | string  | Screen name.

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
success   | bool     | It signifies successful completion of the request

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
 
 `GET http://52.50.150.232/api/deactivate-account`

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
success   | bool     | It signifies successful completion of the request

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
 
 `GET http://52.50.150.232/api/disconnect-social`
 
### Request Parameters

Key         | Place        | Type    | Description
----------- | ------------ | ------- | -----------
social      | query string | string  | can be: "twitter", "facebook", "google"

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
success   | bool     | It signifies successful completion of the request

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
 
 `GET http://52.50.150.232/api/join-to-event`
 
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
success   | bool     | It signifies successful completion of the request

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
 
 `GET http://52.50.150.232/api/leave-event`
 
### Request Parameters

Key         | Place        | Type    | Description
----------- | ------------ | ------- | -----------
id          | query string | integer | id of event

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
success   | bool     | It signifies successful completion of the request

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
    "longitude": 49.4324,
    "chatRoomId": 1,
    "reservedPlaces": 0,
    "hostUsername": "Illia Hapak",
    "hostAvatar": "http://foodizfront.dev/files/images/gprDR_B2dZ_1468578009.jpg",
    "rating": 3.5
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
    "longitude": 49.4324,
    "chatRoomId": 1,
    "reservedPlaces": 0,
    "hostUsername": "Illia Hapak",
    "hostAvatar": "http://foodizfront.dev/files/images/gprDR_B2dZ_1468578009.jpg",
    "rating": 3.5
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
 
 `GET http://52.50.150.232/api/view-archive-events`
 
### Request Parameters

Key  | Place        | Type    | Description
---- | ------------ | ------- | -----------
page | query string | integer | number of page (optional), by default = 1
size | query string | integer | size of page (optional), by default = 10

### Success Response 

 Returns json array of [Event](#event) models.

## (REST) View member events

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
    "longitude": 49.4324,
    "chatRoomId": 1,
    "reservedPlaces": 0,
    "hostUsername": "Illia Hapak",
    "hostAvatar": "http://foodizfront.dev/files/images/gprDR_B2dZ_1468578009.jpg",
    "rating": 3.5
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
    "longitude": 49.4324,
    "chatRoomId": 1,
    "reservedPlaces": 0,
    "hostUsername": "Illia Hapak",
    "hostAvatar": "http://foodizfront.dev/files/images/gprDR_B2dZ_1468578009.jpg",
    "rating": 3.5
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
 
 `GET http://52.50.150.232/api/view-member-events`

### Request Parameters

Key  | Place        | Type    | Description
---- | ------------ | ------- | -----------
page | query string | integer | number of page (optional), by default = 1
size | query string | integer | size of page (optional), by default = 10

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
 
 `GET http://52.50.150.232/api/view-notifications-list`
 
### Request Parameters

Key  | Place        | Type    | Description
---- | ------------ | ------- | -----------
page | query string | integer | number of page (optional), by default = 1
size | query string | integer | size of page (optional), by default = 10

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
    "longitude": 49.4324,
    "chatRoomId": 1,
    "reservedPlaces": 0,
    "hostUsername": "Illia Hapak",
    "hostAvatar": "http://foodizfront.dev/files/images/gprDR_B2dZ_1468578009.jpg",
    "rating": 3.5
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
    "longitude": 49.4324,
    "chatRoomId": 1,
    "reservedPlaces": 0,
    "hostUsername": "Illia Hapak",
    "hostAvatar": "http://foodizfront.dev/files/images/gprDR_B2dZ_1468578009.jpg",
    "rating": 3.5
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
 
 `GET http://52.50.150.232/api/view-own-events`

### Request Parameters

Key  | Place        | Type    | Description
---- | ------------ | ------- | -----------
page | query string | integer | number of page (optional), by default = 1 
size | query string | integer | size of page (optional), by default = 10

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
    "longitude": 49.4324,
    "chatRoomId": 1,
    "reservedPlaces": 0,
    "hostUsername": "Illia Hapak",
    "hostAvatar": "http://foodizfront.dev/files/images/gprDR_B2dZ_1468578009.jpg",
    "rating": 3.5
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
    "longitude": 49.4324,
    "chatRoomId": 1,
    "reservedPlaces": 0,
    "hostUsername": "Illia Hapak",
    "hostAvatar": "http://foodizfront.dev/files/images/gprDR_B2dZ_1468578009.jpg",
    "rating": 3.5
  }
]
```

> Error Response

```json
{
  "success": false,
  "error": "Invalid chat room id"
}
```

Returns all active events in requested area.

### HTTP Request
 
 `GET http://52.50.150.232/api/view-upcoming-events`
 
### Request Parameters

Key         | Place        | Type    | Description
----------- | ------------ | ------- | -----------
chatRoomId  | query string | integer | id of general chat room
page        | query string | integer | number of page (optional), by default = 1
size        | query string | integer | size of page (optional), by default = 10

### Success Response 

 Returns json array of [Event](#event) models.




