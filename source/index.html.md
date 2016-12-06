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
  "statusCode": 204,
  "Authorization": "Bearer ANmpj8rHN88kfiMepBSIuegrxFMulg1475149264",
  "expiresAt": "2016-10-13T11:41:04+00:00",
  "userName": "Valerf",
  "avatarUrl": "http://52.50.150.232/files/images/wDlvX-04N2_1468663003.jpg",
  "id": 28,
  "google": null,
  "twitter": "771681186552934400",
  "facebook": null,
  "colorPrimary": "#1712a3",
  "colorPrimaryDark": "#1cdb23",
  "url": "http://52.50.150.232/activate-account"
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

Key              | Type             | Description
---------------- | ---------------- | -----------
statusCode       | integer          | define success login scenario: 200 - user logged in, 204 - user logged in, but his account deactivated or blocked
Authorization    | string           | token for mobile app
expiresAt        | string           | token expiration date (format: ISO 8601)
username         | string           | user name from social networks
avatarUrl        | string           | url of avatar that resides on server
id               | integer          | id of user
google           | string           | google id
twitter          | string           | twitter id
facebook         | string           | facebook id
colorPrimary     | string           | primary app color
colorPrimaryDark | string           | dark primary app color
url              | string(optional) | if statusCode = 204, then contain url of page with ban info or account activation page 

### Error responses

401

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

### Error responses

401, 500

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
 
### Error responses

400
 
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
    "messageCreated": "2016-08-31T13:31:32+00:00",
    "message": "gtv"
  },
  {
    "isOwner": false,
    "userId": 3,
    "userName": "Eduard Chory",
    "userAvatar": "http://52.50.150.232/files/images/wn5Iof8I0w_1468836563.png",
    "messageId": 10,
    "messageType": "message",
    "messageCreated": "2016-08-31T13:31:32+00:00",
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
    "messageCreated": "2016-08-31T13:31:32+00:00",
    "message": "gtv"
  },
  {
    "isOwner": false,
    "userId": 3,
    "userName": "Eduard Chory",
    "userAvatar": "http://52.50.150.232/files/images/wn5Iof8I0w_1468836563.png",
    "messageId": 10,
    "messageType": "message",
    "messageCreated": "2016-08-31T13:31:32+00:00",
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
    "messageCreated":"2016-08-31T13:31:32+00:00",
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
    "messageCreated":"2016-08-31T13:31:32+00:00",
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
    "url": "v=op3eNwtSGfM",
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
    "messageCreated":"2016-08-31T13:31:32+00:00",
    "videoId":"v=op3eNwtSGfM",
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
url         | body  | string      | url of video(or youtube video id)
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
videoId        | string   | Youtube video id or full url
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
    "messageCreated":"2016-08-31T13:31:32+00:00",
    "event":{
        "id":"1",
        "title":"test",
        "userId":"18",
        "created":"2016-08-31T13:31:32+00:00",
        "updated":"2016-08-31T13:31:32+00:00",
        "date":"2016-08-31T13:31:32+00:00",
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

 Send event into chat.
 
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
  "name": "Bad Request",
  "message": "Status is invalid.",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
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
    "date":"2016-08-31T13:31:32+00:00",
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
  "created": "2016-08-31T13:31:32+00:00",
  "updated": "2016-08-31T13:31:32+00:00",
  "date": "2016-08-31T13:31:32+00:00",
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
  "name": "Bad Request",
  "message": "Invalid Cover Url!",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
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
    "created": "2016-08-31T13:31:32+00:00",
    "username": "Illia Hapak",
    "userAvatar": "http://foodizfront.dev/files/images/gprDR_B2dZ_1468578009.jpg"
  },
  {
    "id": 2,
    "message": "Test comment",
    "created": "2016-08-31T13:31:32+00:00",
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
    "created": "2016-08-31T13:31:32+00:00",
    "updated": "2016-08-31T13:31:32+00:00",
    "date": "2016-08-31T13:31:32+00:00",
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
    "id": 3,
    "username": "Eduard Chory",
    "google": "108334144855221607574",
    "twitter": null,
    "facebook": null,
    "avatarUrl": "http://52.50.150.232/files/images/wn5Iof8I0w_1468836563.png",
    "created": "2016-07-18T10:09:23+00:00",
    "updated": "2016-07-18T10:09:23+00:00",
    "blocked_till": null,
    "blocked_reason": null,
    "online": 0,
    "active": 1,
    "banType": null,
    "admin": 0
  },
  "participants": [
    {
        "id": 3,
        "username": "Eduard Chory",
        "google": "108334144855221607574",
        "twitter": null,
        "facebook": null,
        "avatarUrl": "http://52.50.150.232/files/images/wn5Iof8I0w_1468836563.png",
        "created": "2016-07-18T10:09:23+00:00",
        "updated": "2016-07-18T10:09:23+00:00",
        "blocked_till": null,
        "blocked_reason": null,
        "online": 0,
        "active": 1,
        "banType": null,
        "admin": 0
      }
  ]
}
```

> Error Response

```json
{
  "name": "Bad Request",
  "message": "Event not exist.",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
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
  "name": "Bad Request",
  "message": "User or event not exist",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
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
  "created": "2016-08-31T13:31:32+00:00",
  "updated": "2016-08-31T13:31:32+00:00",
  "date": "2016-08-31T13:31:32+00:00",
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

Adds new rating for event and notify HOST about that.

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

[Push notification](#rate-event)

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
  "created": "2016-08-31T13:31:32+00:00",
  "updated": "2016-08-31T13:31:32+00:00",
  "date": "2016-08-31T13:31:32+00:00",
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
  "name": "Bad Request",
  "message": "Invalid Cover Url!",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

Updating an existing event model.

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

# Models

## Comment

> JSON:

```json
{
    "id": 1,
    "message": "Test comment",
    "created": "2016-08-31T13:31:32+00:00",
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
  "created": "2016-08-31T13:31:32+00:00",
  "updated": "2016-08-31T13:31:32+00:00",
  "date": "2016-08-31T13:31:32+00:00",
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
    "messageCreated": "2016-08-31T13:31:32+00:00",
    "message": "gtv"
}
```

Key            | Type             | Description
-------------- | ---------------- | -----------
isOwner        | bool             | defines that current user is author of the message
userId         | integer          | id of user
userName       | string           | username taken from social
userAvatar     | string           | url of avatar that resides on server
messageId      | integer          | id of message
messageType    | string           | can be: "message", "photo", "video"
messageCreated | string           | date of creation
message        | string(optional) | content of message, if messageType="message"
url            | string(optional) | url photo that resides on server, if messageType="photo"
videoId        | string(optional) | youtube video id, if messageType="video"

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
    "messageCreated": "2016-08-31T13:31:32+00:00",
    "message": "gtv"
}
```

Key            | Type             | Description
-------------- | ---------------- | -----------
isOwner        | bool             | defines that current user is author of the message
userId         | integer          | id of user
userName       | string           | username taken from social
userAvatar     | string           | url of avatar that resides on server
messageId      | integer          | id of message
messageType    | string           | can be: "message", "photo", "video", "event"
messageCreated | string           | date of creation
event          | json object      | [Event](#event) model, if messageType="event"
message        | string(optional) | content of message, if messageType="message"
url            | string(optional) | url photo that resides on server, if messageType="photo"
videoId        | string(optional) | youtube video id, if messageType="video"

## Push Notification

> JSON:

```json
{
    "id": 1,
    "message": "By the event joined by Illia Hapak",
    "action": "Join to event.",
    "created": "2016-08-31T13:31:32+00:00"
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
      "id": 3,
      "username": "Eduard Chory",
      "google": "108334144855221607574",
      "twitter": null,
      "facebook": null,
      "avatarUrl": "http://52.50.150.232/files/images/wn5Iof8I0w_1468836563.png",
      "created": "2016-07-18T10:09:23+00:00",
      "updated": "2016-07-18T10:09:23+00:00",
      "blocked_till": null,
      "blocked_reason": null,
      "online": 0,
      "active": 1,
      "banType": null,
      "admin": 0,
      "email": "example@example.com"
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
online         | bool     | Shows that user online
active         | bool     | Shows that user account is active
banType        | string   | can be: "permanent_ban", "daily_ban", "temporary_ban"
admin          | bool     | Shows that user is admin
email          | string   | user email selected from social, can be null

# Push notifications

## Ban
Can be received by all users(except moderators).
Reports the user that his account banned.
<aside class="notice">
 When redirecting user by banInfo link, http header should contain user access token.
</aside>

### Extra fields
Key     | Type   | Description
------- | ------ | -----------
action  | string | Define push type
banInfo | string | Link on web page with info about ban

<aside class="notice">
 action contain "ban"
</aside>

## Cancel event

Can be received by participants of events.
Reports the user about the event cancellation.

### Extra fields
Key     | Type    | Description
------- | ------- | -----------
action  | string  | Define push type
id      | integer | id of event

<aside class="notice">
 action contain "cancel_event"
</aside>

## Cancel reservation

Can be received only by HOSTs.
Reports HOST about the user which canceled reservation in event.

### Extra fields
Key         | Type    | Description
----------- | ------- | -----------
action      | string  | Define push type
eventId     | integer | id of event
userId      | integer | id of user that canceled reservation
explanation | string  | explanation why participant leave event


<aside class="notice">
 action contain "cancel_reservation"
</aside>

## Close event

Can be received by participants of events.
Reports the user about closing of event.

### Extra fields
Key     | Type    | Description
------- | ------- | -----------
action  | string  | Define push type
eventId | integer | id of event

<aside class="notice">
 action contain "close_event"
</aside>

## Comment event

Can be received only by HOSTs of events.
Reports the HOST about new comments to finished events.

### Extra fields
Key        | Type    | Description
---------- | ------- | -----------
action     | string  | Define push type
eventId    | integer | id of event
userName   | string  | username of user that post a comment
userAvatar | string  | avatar url of user that post a comment 

<aside class="notice">
 action contain "comment_event"
</aside>

## New chat message

Can be received by participants of chat rooms which are offline.
Reports user about the new messages in his current chat room.

### Extra fields
Key         | Type    | Description
----------- | ------- | -----------
action      | string  | Define push type
chatId      | integer | id of chat room
messageType | string  | can be: "event" or "general"
messageId   | integer | id of chat room message
userName    | string  | username of user that post a message
userAvatar  | string  | avatar url of user that post a message
eventId     | integer | id of event assigned to chat room, null if it's general chat room message

<aside class="notice">
 action contain "post_message"
</aside>

## New event in your area

Can be received by all users.
Reports user about new event created by another user in the same chat room as receiver.

### Extra fields
Key     | Type    | Description
------- | ------- | -----------
action  | string  | Define push type
eventId | integer | id of event

<aside class="notice">
 action contain "new_event"
</aside>

## New participant of event

Can be received by participants of events and HOST of event.
Reports by the events of the new member of the same event.

### Extra fields
Key        | Type    | Description
---------- | ------- | -----------
action     | string  | Define push type
eventId    | integer | id of event
userId     | integer | id of user that joined to event
userName   | string  | username of user that joined to event
userAvatar | string  | avatar url of user that joined to event


<aside class="notice">
 action contain "new_participant"
</aside>

## Private message from moderator

Can be received by all users.
Message sent from CMS by moderator.

### Extra fields
Key     | Type   | Description
------- | ------ | -----------
action  | string | Define push type

<aside class="notice">
 action contain "private_message"
</aside>

## Rate event

Can be received only by HOSTs of events.
Notify HOST about new rating of held event.

### Extra fields
Key            | Type    | Description
-------------- | ------- | -----------
action         | string  | Define push type
eventId        | integer | event id
username       | string  | username of user that put rating
userAvatarUrl  | string  | avatar url of user that put rating

<aside class="notice">
 action contain "repeat_event"
</aside>

## Repeat event

Can be received by participants of events.
Reports the user about event recurrence.

### Extra fields
Key     | Type    | Description
------- | ------- | -----------
action  | string  | Define push type
id      | integer | event id

<aside class="notice">
 action contain "repeat_event"
</aside>

## Un ban

Can be received by all users(except moderators).
Reports the user that his account was unlocked.

### Extra fields
Key     | Type   | Description
------- | ------ | -----------
action  | string | Define push type

<aside class="notice">
 action contain "un_ban"
</aside>

## Update event

Can be received by participants of event.
Reports participant about update of event.

### Extra fields
Key         | Type    | Description
----------- | ------- | -----------
action      | string  | Define push type
eventId     | integer | id of event


<aside class="notice">
 action contain "update_event"
</aside>

# User settings and profile

## (REST) Change avatar

> Success Response

```json
{
  "success": true
}
```

> Error Response

```json
{
  "name": "Not Found",
  "message": "Image not found",
  "code": 0,
  "status": 404,
  "type": "yii\\web\\NotFoundHttpException"
}
```

Change user avatar to image which resides on server by url.

### HTTP Request
 
 `GET http://52.50.150.232/api/change-avatar`

### Request Parameters

Key      | Place        | Type    | Description
-------- | ------------ | ------- | -----------
url      | query string | string  | url of image that resides on server

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
success   | bool     | It signifies successful completion of the request

### Error responses

401, 403, 404, 466, 500

## (REST) Change username

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
  "message": "Username should be no longer then 50 symbols",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

Change user avatar to image which resides on server by url.

### HTTP Request
 
 `GET http://52.50.150.232/api/change-username`

### Request Parameters

Key         | Place        | Type    | Description
----------- | ------------ | ------- | -----------
newUsername | query string | string  | New username

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
success   | bool     | It signifies successful completion of the request

### Error responses

400, 401, 403, 466

## (REST) Connect social

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
  "message": "User with this social ID exist",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

Connect social network to user account.  
If user with this social id exist it generates BadRequestHttpException.

### HTTP Request
 
 `POST http://52.50.150.232/api/connect-social`
 
### Request Parameters

Key         | Place  | Type    | Description
----------- | ------ | ------- | -----------
social      | body   | string  | can be: "twitter", "facebook", "google"
token       | body   | string  | social token
secret      | body   | string  | if social="twitter"

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
  "name": "Bad Request",
  "message": "Invalid social network!",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

Disconnect selected social network from user account.

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
  "name": "Bad Request",
  "message": "Event not exist.",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
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
  "name": "Bad Request",
  "message": "The user does not take part in the event.",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

Remove user from participants of event and send push notification to HOST with explanation why user leave event.

### HTTP Request
 
 `GET http://52.50.150.232/api/leave-event`
 
### Request Parameters

Key         | Place        | Type    | Description
----------- | ------------ | ------- | -----------
id          | query string | integer | id of event
explanation | query string | string  | explanation why participant leave event, not empty and not more than 255 characters

### Success Response model

Key       | Type     | Description
--------- | -------- | -----------
success   | bool     | It signifies successful completion of the request

[Push notification](#cancel-reservation)

## (REST)Refresh connection

> Success Response:

```json
{
    "colorPrimary": "#1712a3",
    "colorPrimaryDark": "#1cdb23"
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
 Return actual app colors.

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

Key              | Type     | Description
---------------- | -------- | -----------
colorPrimary     | string   | primary app color
colorPrimaryDark | string   | dark primary app color

## (REST) Remove avatar

> Success Response

```json
{
  "defaultAvatar": "http://foodizfront.dev/files/default/male.png"
}
```

> Error Response

```json
{
  "name": "Bad Request",
  "message": "You can't delete default avatar",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

Remove user avatar and set default avatar. Default avatar can't be removed!  
Relative url of default avatar /files/default/male.png

### HTTP Request
 
 `GET http://52.50.150.232/api/remove-avatar`

### Success Response model

Key           | Type   | Description
------------- | ------ | -----------
defaultAvatar | string | Url of default avatar image which resides on server

### Error responses

400, 401, 403, 466, 500

## (REST) View archive events

> Success Response

```json
[
  {
    "id": 4,
    "title": "Uzhgorod first event",
    "userId": 1,
    "created": "2016-08-31T13:31:32+00:00",
    "updated": "2016-08-31T13:31:32+00:00",
    "date": "2016-08-31T13:31:32+00:00",
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
    "created": "2016-08-31T13:31:32+00:00",
    "updated": "2016-08-31T13:31:32+00:00",
    "date": "2016-08-31T13:31:32+00:00",
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
    "created": "2016-08-31T13:31:32+00:00",
    "updated": "2016-08-31T13:31:32+00:00",
    "date": "2016-08-31T13:31:32+00:00",
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
    "created": "2016-08-31T13:31:32+00:00",
    "updated": "2016-08-31T13:31:32+00:00",
    "date": "2016-08-31T13:31:32+00:00",
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

## (REST) View not rated by user events

> Success Response

```json
[
  {
    "id": 4,
    "title": "Uzhgorod first event",
    "userId": 1,
    "created": "2016-08-31T13:31:32+00:00",
    "updated": "2016-08-31T13:31:32+00:00",
    "date": "2016-08-31T13:31:32+00:00",
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
    "created": "2016-08-31T13:31:32+00:00",
    "updated": "2016-08-31T13:31:32+00:00",
    "date": "2016-08-31T13:31:32+00:00",
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

Returns all finished and not rated by user events in which user participated before.

### HTTP Request
 
 `GET http://52.50.150.232/api/view-not-rated-events`

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
    "created": "2016-08-31T13:31:32+00:00"
  },
  {
    "id": 2,
    "message": "User Illia Hapak canceled reservation.",
    "action": "Leave event.",
    "created": "2016-08-31T13:31:32+00:00"
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
    "created": "2016-08-31T13:31:32+00:00",
    "updated": "2016-08-31T13:31:32+00:00",
    "date": "2016-08-31T13:31:32+00:00",
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
    "created": "2016-08-31T13:31:32+00:00",
    "updated": "2016-08-31T13:31:32+00:00",
    "date": "2016-08-31T13:31:32+00:00",
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
    "created": "2016-08-31T13:31:32+00:00",
    "updated": "2016-08-31T13:31:32+00:00",
    "date": "2016-08-31T13:31:32+00:00",
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
    "created": "2016-08-31T13:31:32+00:00",
    "updated": "2016-08-31T13:31:32+00:00",
    "date": "2016-08-31T13:31:32+00:00",
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
  "name": "Bad Request",
  "message": "Invalid chat room id",
  "code": 0,
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
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




