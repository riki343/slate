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
    "message": "some message"
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
    "message": "some error message"
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

> Success Response:

```json
[
    {
        "id": 5,
        "title": "some title",
        "messages": [
            {
                "chatID": 1,
                "userID": 12,
                "username": "John Doe",
                "avatar": "http://127.0.0.1/uploads/images/some_image.png",
                "type": "message",
                "photoID": 251,
                "photoUrl": "http://127.0.0.1/uploads/images/some_other_image.jpg",
                "videoID": 85,
                "videoUrl": "https://www.youtube.com/watch?v=Yr0o6oIw3Nw",
                "created": "2005-08-15T15:52:01+0000",
                "updated": "2005-08-15T15:52:01+0000",
                "eventID": 23,
                "event": {
                    "id": 23,
                    "title": "Some event",
                    "userID": 18,
                    "created": "2005-08-15T15:52:01+0000",
                    "updated": "2005-08-15T15:52:01+0000",
                    "status": "active"
                }
            }
        ]
    }
]
```

> Error Response

```json
{
    "message": "some message"
}
```

Get all general chats in your area

### HTTP Request

`POST http://52.50.150.232/api/chats/around`

### Request Parameters

Parameter   | Place | Type      | Description
----------- | ----- | --------- | -----------
address     | body  | string    | Address from geocoder
latitude    | body  | float     | latitude from geocoder
longitude   | body  | float     | longitude from geocoder

### Success Response model

Parameter   | Type     | Description
----------- | -------- | -----------
id          | integer  | id of chat room
title       | string   | chat title
messages    | array    | array with chat messages
chatID      | integer  | id chat that message related to
userID      | integer  | id of user that posted message
message     | string   | 
username    | string   | username of user that posted message
avatar      | string   | avatar of user that posted message
type        | string   | type of message, it can be: image, video, event, message
photoID     | integer  | id of posted photo. It's NOT NULL only if type = "image"
photoUrl    | string   | url of posted photo. It's NOT NULL only if type = "image"
videoID     | integer  | id of posted video. It's NOT NULL only if type = "video"
videoUrl    | string   | url of posted video. It's NOT NULL only if type = "video"
eventID     | integer  | id of posted event. It's NOT NULL only if type = "event"
event       | object   | event object. It's NOT NULL only if type = "event"
status      | string   | this value indicate's event status. values can be: "active", "finished", "closed"
created     | string   | creation date (format: ISO 8601)
updated     | string   | last updated (format: ISO 8601)



## (WS) Connect to chat

> Success Response

```json
{
    "message": "Connected"
}
```

### WEBSOCKETS Request

`ws://52.50.150.232`

### Join room

`general.{country}.{city}`

### Request Parameters

Parameter   | Place | Type      | Description
----------- | ----- | --------- | -----------
country     | json  | string    | country that the chat is related to
city        | json  | string    | city that the chat is related to



## (WS) Post message in chat

> Request body

```json
{
    "action": "general.post.message",
    "message": "some message",
    "token": "dajkwdhklawdwiuaye73yurha322ueudho82uh2e2lie"
}
```

> Success Response

```json
{
    "chatID": 1,
    "userID": 12,
    "username": "John Doe",
    "avatar": "http://127.0.0.1/uploads/images/some_image.png",
    "message": "Some message",
    "type": "message",
    "photoID": null,
    "photoUrl": null,
    "videoID": null,
    "videoUrl": null,
    "created": "2005-08-15T15:52:01+0000",
    "updated": "2005-08-15T15:52:01+0000",
    "eventID": null,
    "event": null
}
```

### WEBSOCKETS request

`ws://52.50.150.232`

### Request Parameters

Parameter   | Place | Type      | Description
----------- | ----- | --------- | -----------
action      | json  | string    | action that backend may to do
message     | json  | string    | the message that you want to post (max 500 characters)
token       | json  | string    | application token 

### Success Response model

Parameter   | Type     | Description
----------- | -------- | -----------
id          | integer  | message id
chatID      | integer  | id chat that message related to
userID      | integer  | id of user that posted message
message     | string   | 
username    | string   | username of user that posted message
avatar      | string   | avatar of user that posted message
type        | string   | type of message, it can be: image, video, event, message
photoID     | null     | id of posted photo. It's NOT NULL only if type = "image"
photoUrl    | null     | url of posted photo. It's NOT NULL only if type = "image"
videoID     | null     | id of posted video. It's NOT NULL only if type = "video"
videoUrl    | null     | url of posted video. It's NOT NULL only if type = "video"
eventID     | null     | id of posted event. It's NOT NULL only if type = "event"
event       | null     | event object. It's NOT NULL only if type = "event"
status      | string   | this value indicate's event status. values can be: "active", "finished", "closed"
created     | string   | creation date (format: ISO 8601)
updated     | string   | last updated (format: ISO 8601)