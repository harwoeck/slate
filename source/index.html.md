---
title: vikebot Wiki

language_tabs: # must be one of https://git.io/vQNgJ
- shell: cURL

search: true
---
# Introduction

Vikebot is a competitive online coding game. This site contains all the informations needed to interact with the complete server infrastructure.

# REST

## Test access

```shell
curl -X GET "https://api.vikebot.com/v1/test" \
  -H "cache-control: no-cache"
```

> The above command returns JSON structured like this:

```json
{
  "response": "ok"
}
```

## User - Get your account

```shell
curl -X GET "https://api.vikebot.com/v1/user/get" \
  -H "authorization: bearer JWT" \
  -H "cache-control: no-cache"
```

> The above command returns JSON structured like this:

```json
{
  "id": 100000,
  "permission": 1,
  "permission_string": "default",
  "username": "jsmith",
  "name": "Jon Smith",
  "emails": [

  ],
  "bio": "I'm a test account used in the documentations of vikebot",
  "location": "Wels, Austria",
  "web": [
    "https://vikebot.com"
  ],
  "company": "@vikebot",
  "social": {
    "github":"https://github.com/vikebot"
  }
}
```

## User - Get by ID

```shell
curl -X GET "https://api.vikebot.com/v1/user/get/id/USERID" \
  -H "cache-control: no-cache"
```

> The above command returns JSON structured like this:

```json
{
  "id": 100000,
  "permission": 1,
  "permission_string": "default",
  "username": "jsmith",
  "name": "Jon Smith",
  "emails": [

  ],
  "bio": "I'm a test account used in the documentations of vikebot",
  "location": "Wels, Austria",
  "web": [
    "https://vikebot.com"
  ],
  "company": "@vikebot",
  "social": {
    "github":"https://github.com/vikebot"
  }
}
```

<aside class="notice">
  If you provide the <code>Authorization</code>-Header we also will return <code>joined</code> rounds. If this header is not set the response will only include <code>open</code> rounds.
</aside>

## User - Get by Username

```shell
curl -X GET "https://api.vikebot.com/v1/user/get/username/USERNAME" \
  -H "cache-control: no-cache"
```

> The above command returns JSON structured like this:

```json
{
  "id": 100000,
  "permission": 1,
  "permission_string": "default",
  "username": "jsmith",
  "name": "Jon Smith",
  "emails": [

  ],
  "bio": "I'm a test account used in the documentations of vikebot",
  "location": "Wels, Austria",
  "web": [
    "https://vikebot.com"
  ],
  "company": "@vikebot",
  "social": {
    "github":"https://github.com/vikebot"
  }
}
```

<aside class="notice">
  If you provide the <code>Authorization</code>-Header we also will return <code>joined</code> rounds. If this header is not set the response will only include <code>open</code> rounds.
</aside>

## User - Update

```shell
curl -X POST "https://api.vikebot.com/v1/user/update" \
  -H "authorization: bearer JWT" \
  -H "cache-control: no-cache" \
  -H "content-type: application/json" \
  -d '{
    "username": "jsmith",
    "name": "Jon Smith",
    "email": "jon.smith@gmail.com",
    "bio": "",
    "location": "",
    "company": ""
  }'
```

> The above command returns JSON structured like this:

```json
{
  "err": null
}
```

## Register - Confirm

```shell
curl -X POST "https://api.vikebot.com/v1/register/confirm" \
  -H "authorization: bearer JWT" \
  -H "cache-control: no-cache" \
  -H "content-type: application/json" \
  -d '{
    "username": "jsmith",
    "name": "Jon Smith",
    "email": "jon.smith@gmail.com",
    "bio": "",
    "location": "",
    "company": ""
  }'
```

## Round - Active

```shell
curl -X GET "https://api.vikebot.com/v1/round/active" \
  -H "cache-control: no-cache"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Test Game",
    "wallpaper": "https://goo.gl/uCyW7n",
    "joined": 0,
    "min": 10,
    "max": 40,
    "starttime": "2017-12-28T08:26:13Z",
    "status": 1
  }
]
```

## Round - Join

```shell
curl -X POST "https://api.vikebot.com/v1/round/join/ROUNDID" \
  -H "authorization: bearer JWT" \
  -H "cache-control: no-cache"
```

> The above command returns JSON structured like this:

```json
{
  "err": null
}
```

## Roundentry - Active

```shell
curl -X POST "https://api.vikebot.com/v1/round/join/ROUNDID" \
  -H "authorization: bearer JWT" \
  -H "cache-control: no-cache"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Test Game",
    "wallpaper": "https://goo.gl/uCyW7n",
    "joined": 0,
    "min": 10,
    "max": 40,
    "starttime": "2017-12-28T08:26:13Z",
    "status": 1,
    "authtoken": "",
    "watchtoken": ""
  }
]
```

## Roundentry - Connectinfo

```shell
curl -X GET "https://api.vikebot.com/v1/roundentry/connectinfo/YOUR-AUTHTOKEN" \
  -H "cache-control: no-cache"
```

> The above command returns JSON structured like this:

```json
{
  "ticket": "rlcibQmvurKUVGXV",
  "aes_key": "ak1z7OHUPRvX8DJsnm31NDfQmYE0hfNmFH7paT4WkUQ=",
  "ipv4": "127.0.0.1",
  "ipv6": "::1/128",
  "port": 2400
}
```

The only info a client has after joining a round is his `authtoken`. A 16-charactar long token containing only digits and letters (upper and lower case). To get information about the game server (e.g. host address and port) the client needs to exchange his `authtoken`.

This is done by calling the `roundentry/connectinfo` endpoint with a `HTTP GET` request.

Returned is the `ticket` used for authentication on the game server, an `AES256`-bit key encoded in base64, the addresses and ports of the game server.
