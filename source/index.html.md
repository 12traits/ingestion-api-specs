---
title: 12traits Ingestion API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='https://12traits.com'>Sign Up to get API Key</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the 12traits Ingestion API!

The 12traits API is a [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) API, which accepts and returns JSON-encoded data, and uses standard HTTP response codes, authentication, and verbs.

# Authentication

The 12traits API uses API Keys to authenticate requests. You can view and manage your API Keys in the 12traits Dashboard.

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

> Send your API Key with the request.

```shell
curl -XPOST -H "Authorization: Bearer <API_KEY>" https://api.12traits.com
```

# API limits

- It is possible to push up to 1000 multiple items at the same time.

# List your games

You will need to know your game ID in order to push data. You can get this ID by calling this endpoint.

> Request

```shell
curl https://api.12traits.com/v1/games
```

> Example response

```js
{
  "code": 200,
  "message": "",
  "data": [
    { "game_id": "123", "name": "Fortnite" }
  ]
}
```

# Push game actions

> Push the player’s actions.

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "actions": [ { "timestamp": 1000000000, "player_id": "1001", "action_id": "2002", "action_name": "buy_item", "action_params": [ {"key": "item_id", "value": "3003" } ] ] }'
https://api.12traits.com/v1/<game_id>/abc/actions
```

### Action

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
timestamp|Timestamp||YYYY-MM-DD HH:MM:SS
player_id|string|Yes|Unique player ID
action_id|string|Yes|Action ID specific to the game
action_name|string|Yes|User-friendly name, for example: "buy_item", "start_fight"
action_params|list\<ActionParam\>| |Map of action params

### ActionParam

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
key|string|Yes|Action param key
value|string|Yes|Action param value

> Example response

```js
{
  "code": 201,
  "message": "50 action(s) have been successfully ingested",
  "data": null
}
```

# Push logged in players

> Push information when players have logged in into the game.

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "players": [ { "timestamp": 1000000000, "player_id": "1001", "level": 10, "device": "iPhone X", "language": "en-US", "platform": "Apple" }'
https://api.12traits.com/v1/games/<game_id>/player-logins
```

### Player

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
timestamp|Timestamp||YYYY-MM-DD HH:MM:SS
player_id|string|Yes|Unique player ID
level|int||Player's level or rank in the game
device|string||Device player is using. e.g. iPad, iPhone X
platform|string||Platform player is using, e.g. Steam
language|string||Player's language in [IETF format](https://en.wikipedia.org/wiki/IETF_language_tag)

> Example response

```js
{
  "code": 201,
  "message": "50 player(s) have been successfully ingested",
  "data": null
}
```

# Push Purchases

> Push information related to the real-money purchases made by each user.

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "purchases": [ { "timestamp": 1000000000, "player_id": "1001", "item_id": "2002", "count": 1, "price": 25.50, "platform": "Steam" }'
https://api.12traits.com/v1/games/<game_id>/purchases
```

### Purchase

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
timestamp|Timestamp||YYYY-MM-DD HH:MM:SS
player_id|string|Yes|Unique player ID
item_id|string|Yes|In game Item ID
count|int|Yes|Number of items purchased
price|float|Yes|Total money spent on the purchase
platform|string||Platform player is using, e.g. Steam

> Example response

```js
{
  "code": 201,
  "message": "50 purchase(s) have been successfully ingested",
  "data": null
}
```