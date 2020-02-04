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

# REST API Endpoints

We value security, so only HTTPS is supported.

## Production

[https://api.12traits.com](https://api.12traits.com)

## Sandbox

[https://sandbox.api.12traits.com](https://sandbox.api.12traits.com)

- Use the Sandbox endpoint during the implementation phase.
- Switch to the Production endpoint once the testing is completed.

# Authentication

The 12traits API uses API Keys to authenticate requests. You can view and manage your API Keys in the 12traits Dashboard. Each game has its own API Key.

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

> Send your API Key with the request.

```shell
curl -XPOST -H "Authorization: Bearer <API_KEY>" https://api.12traits.com/v1/games/...
```

# API limits

- It is possible to push up to 1000 multiple items at the same time.

# Postman Collection

<a href="https://www.getpostman.com/" target="_blank">Postman</a> is helpful when first integrating with the 12traits API. Postman is a powerful tool that will help you call and debug specific response data, as well as any error messages.

## How to use 12traits API Postman Collection

1. Go to <a href="https://documenter.getpostman.com/view/6074213/SVtZvRPm">https://documenter.getpostman.com/view/6074213/SVtZvRPm</a>
2. Click "Run in Postman"
3. Set your environment variables in Postman App

### 12traits API variables

- **BASE_URL** - Production - https://api.12traits.com, or sandbox - https://sandbox.api.12traits.com
- **API_KEY** - Paste your game API Key here

Now you are ready to test 12traits API endpoints from Postman!

# Push Player Custom Data

You can send any custom player data from your own surveys by uploading a CSV file in the [following format](https://storage.googleapis.com/12traits/12traits_custom_data_template.csv).

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: multipart/form-data" \
-F file=@2019-09-23.csv \
https://api.12traits.com/v1/games/player-attributes/csv
```

# Push Player KPIs as CSV

You can combine daily KPIs for all players in a single CSV file and send it at once. The CSV file should follow the [following format](https://storage.googleapis.com/12traits/kpis_template.csv).

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: multipart/form-data" \
-F file=@2019-09-23.csv \
https://api.12traits.com/v1/games/kpis/csv
```

# Push Game Actions

Push the player’s actions. This can be any set of actions that your game generates.

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "actions": [ { "timestamp": "2019-01-01 15:04:05", "player_id": "1001", "action_id": "2002", "action_name": "buy_item", "action_params": [ {"key": "item_id", "value": "3003" } ] } ] }' \
https://api.12traits.com/v1/games/actions
```

### Action

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
timestamp|Timestamp||YYYY-MM-DD HH:MM:SS in UTC
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
  "message": "50 actions have been successfully ingested",
  "data": null
}
```

# Push Logged In Players

Push information when players log into the game.

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "players": [ { "timestamp": "2019-01-01 15:04:05", "player_id": "1001", "clan_id": "2002", "level": 10, "device": "iPhone X", "country": "US", "language": "en-US", "platform": "Apple", "register_date": "2018-11-14", "last_login_timestamp": "2019-01-01 11:10:06" } ] }' \
https://api.12traits.com/v1/games/player-logins
```

### Player

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
timestamp|Timestamp||YYYY-MM-DD HH:MM:SS in UTC
player_id|string|Yes|Unique player ID
clan_id|string||Clan/guild/organization the user belongs to
level|int||Player's level or rank in the game
device|string||Device player is using. e.g. iPad, iPhone X
country|string||Country from where player has logged in in ISO Alpha-2 format (2 letters)
platform|string||Platform player is using, e.g. Steam
language|string||Player's language in [IETF format](https://en.wikipedia.org/wiki/IETF_language_tag)
register_date|string||YYYY-MM-DD
last_login_timestamp|Timestamp||YYYY-MM-DD HH:MM:SS in UTC

> Example response

```js
{
  "code": 201,
  "message": "50 players have been successfully ingested",
  "data": null
}
```

# Push Purchases

Push information related to the real-money purchases made by each player.

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "purchases": [ { "timestamp": "2019-01-01 15:04:05", "player_id": "1001", "item_id": "2002", "count": 1, "price": 25.50, "platform": "Steam" } ] }' \
https://api.12traits.com/v1/games/purchases
```

### Purchase

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
timestamp|Timestamp||YYYY-MM-DD HH:MM:SS in UTC
player_id|string|Yes|Unique player ID
item_id|string|Yes|In game Item ID
count|int|Yes|Number of items purchased
price|float|Yes|Total money spent on the purchase
platform|string||Platform player is using, e.g. Steam

> Example response

```js
{
  "code": 201,
  "message": "50 purchases have been successfully ingested",
  "data": null
}
```

# Push Virtual Purchases

Push information related to in-game virtual currency purchases made by each player.

Please note: If an item is bought by first converting real money to in-game money, it should appear in both purchases and virtual purchases.

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "virtual_purchases": [ { "timestamp": "2019-01-01 15:04:05", "player_id": "1001", "item_id": "2002", "count": 1, "virtual_price": 25.50, "currency": "gold" } ] }' \
https://api.12traits.com/v1/games/virtual-purchases
```

### VirtualPurchase

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
timestamp|Timestamp||YYYY-MM-DD HH:MM:SS in UTC
player_id|string|Yes|Unique player ID
item_id|string|Yes|In game Item ID
count|int|Yes|Number of items purchased
virtual_price|float|Yes|Total virtual currency spent on the purchase
currency|string|Yes|Currency of the purchase, e.g. "gold","tokens","crowns"

> Example response

```js
{
  "code": 201,
  "message": "50 virtual purchases have been successfully ingested",
  "data": null
}
```

# Push Level Ups

Push level-ups for each player. Levels refer to game-content levels, i.e. levels that represent the progress of a user through the game. These can have different names (tiers/stages/progress/ranks, etc.), these should always be increasing.

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "level_ups": [ { "timestamp": "2019-01-01 15:04:05", "player_id": "1001", "level": 10 } ] }' \
https://api.12traits.com/v1/games/level-ups
```

### LevelUp

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
timestamp|Timestamp||YYYY-MM-DD HH:MM:SS in UTC
player_id|string|Yes|Unique player ID
level|int|Yes|New level after level-up

> Example response

```js
{
  "code": 201,
  "message": "50 level-ups have been successfully ingested",
  "data": null
}
```

# Push Items

Push detailed information about each item.

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "items": [ { "item_id": "1001", "price": 25.50, "currency": "USD", "name": "Axe", "type": "weapon" } ] }' \
https://api.12traits.com/v1/games/items
```

### Item

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
item_id|string|Yes|Unique item ID
price|float|Yes|Price of the item
currency|string|Yes|Currency of the price, e.g. "JPY", "CNY", "USD", "gold"
name|string|Yes|Item name
type|string||Item type, e.g. "weapon"

> Example response

```js
{
  "code": 201,
  "message": "50 items have been successfully ingested",
  "data": null
}
```

# Push Social Dynamics

Push information on the social dynamics of the game, i.e. interactions between players.

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "social_dynamics": [ { "timestamp": "2019-01-01 15:04:05", "player_id": "1001", "target_player_id": "1002", "connection": "linked", "interaction": "friend" } ] }' \
https://api.12traits.com/v1/games/social-dynamics
```

### SocialDynamics

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
timestamp|Timestamp||YYYY-MM-DD HH:MM:SS in UTC
player_id|string|Yes|Unique player ID
target_player_id|string|Yes|ID of the player with whom a link is made or who is unlinked
connection|string|Yes|"linked" or "unllinked"
interaction|string|Yes|"friend" or "party"

> Example response

```js
{
  "code": 201,
  "message": "50 social dynamics have been successfully ingested",
  "data": null
}
```

# PlayFab Webhook Integration

If you are using PlayFab you are able to integrate it with 12traits by using [PlayFab Webhooks](https://api.playfab.com/docs/tutorials/landing-analytics/webhooks). You can simply configure PlayFab to send all events to 12traits by using this endpoint.

> Endpoint URL. Replace **:api_key** with your game API Key.

```shell
https://api.12traits.com/v1/games/playfab?api_key=:api_key
```

### How to configure a PlayFab webhook

1. Go to PlayFab Analytics
2. Open "Webhooks" tab
3. Click "New Webhook"
4. Give it a name
5. Set Endpoint URL