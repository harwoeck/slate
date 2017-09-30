---
title: vikebot Wiki

language_tabs: # must be one of https://git.io/vQNgJ
- shell: cURL

search: true
---

# Introduction
Vikebot is a competitive online coding game. This site contains all the informations needed to interact with the complete server infrastructure.

# User

## Get your account
```shell
curl -X GET "https://api.vikebot.com/v1/user/get" \
  -H "authorization: bearer JWT" \
  -H "cache-control: no-cache"
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "permission": "verified",
  "username": "jsmith",
  "name": "Jon Smith",
  "email": "jon.smith@gmail.com",
  "bio": "I'm a test account used in the documentations of vikebot",
  "location": "Wels, Austria",
  "company": "@vikebot"
}
```

## Update your account
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


# Register for a game

## List all lobbies
```shell
curl -X GET "https://api.vikebot.com/v1/lobby/list" \
  -H "authorization: bearer JWT" \
  -H "cache-control: no-cache"
```

> The above command returns JSON structured like this:

```json
{
  "joined": [
     {
       "id": 4560,
       "name": "",
       "wallpaper": "",
       "joined": 17,
       "min": 10,
       "max": 20,
       "starttime": "2017-09-30 18:00:00",
       "authtoken": "",
       "watchtoken": ""
     }
  ],
  "open": [
    {
      "id": 4561,
      "name": "dust",
      "wallpaper": "",
      "joined": 4,
      "min": 7,
      "max": 15,
      "starttime": "2017-09-30 18:00:00"
    }
  ]
}
```

<aside class="notice">
  If you provide the <code>Authorization</code>-Header we also will return <code>joined</code> rounds. If this header is not set the response will only include <code>open</code> rounds.
</aside>


## Join a lobby
```shell
curl -X POST "https://api.vikebot.com/v1/lobby/join/ROUND-ID" \
  -H "authorization: bearer JWT" \
  -H "cache-control: no-cache"
```

> The above command returns JSON structured like this:

```json
{
  "err": null
}
```

### Errors
Code | Message
---- | -------
1000 | Roundid must be a int32
1001 | Insufficient permissions. You need at least 'Verified' status
1002 | Internal server error. We apologize for the inconvenience
1003 | Round specified by id doesn't exist
1004 | You already joined this game

## Exchange your authtoken 
```shell
curl -X GET "https://api.vikebot.com/v1/lobby/exchange/YOUR-AUTHTOKEN" \
  -H "cache-control: no-cache"
```

> The above command returns JSON structured like this:

```json
{
  "ticket": "rlcibQmvurKUVGXV",
  "aeskey": "ak1z7OHUPRvX8DJsnm31NDfQmYE0hfNmFH7paT4WkUQ=",
  "aesiv": "RQrFKB6UgyqxeXxUcCToDg==",
  "ipv4": "127.0.0.1",
  "ipv6": "::1/128",
  "port": 2400
}
```

The only info a client has after joining a round is his `authtoken`. A 16-charactar long key containing only digits and letters (upper and lower case). To get information about the game server (e.g. host address and port) the client needs to exchange his `authtoken`.

This is done calling the `roundticket` endpoint with a `HTTP GET`.


### Response

Field | Description
----- | -----------
ticket | A roundticket used during the login procedure to the gameserver. See <a href="">Writing a new SDK</a>.
aeskey | A 256-bit key encoded in `base64`.
aesiv | A 128-bit **Initialisation-Vector** used by `AES-GCM`.
ipv4 | The IPv4 address of the gameserver hosting this round
ipv6 | The IPv6 address of the gameserver hosting this round
port | The port to connect to for your gameserver.
