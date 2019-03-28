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

- Use the Sandbox endpoint and Sandbox API Keys during the implementation phase.
- Switch to the Production endpoint and Production API Keys once the testing is completed.

# Authentication

The 12traits API uses API Keys to authenticate requests. You can view and manage your API Keys in the 12traits Dashboard.

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

> Send your API Key with the request.

```shell
curl -XPOST -H "Authorization: Bearer <API_KEY>" https://api.12traits.com/v1/games/...
```

# API limits

- It is possible to push up to 1000 multiple items at the same time.

# Postman Collection

[Postman](https://www.getpostman.com/) can be very helpful when you just started an integration with 12traits API. With Postman you're able to call and debug the specific response data and any error messages is a powerful way.

## How to use 12traits API Postman Collection

1. Open Postman
2. Click Import -> Import from link
3. Paste the following URL: [https://www.getpostman.com/collections/9c1dc99852e00ff5c8b6](https://www.getpostman.com/collections/9c1dc99852e00ff5c8b6)
4. Set your environment variables.

### 12traits API variables

- **API_KEY** - Paste your API Key here

Now you are ready to test 12traits API endpoints from Postman!

# Push game actions

Push the player’s actions. It can be any set of actions your game generates.

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

# Push logged in players

Push information when players have logged in into the game.

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "players": [ { "timestamp": "2019-01-01 15:04:05", "player_id": "1001", "clan_id": "2002", "level": 10, "device": "iPhone X", "country": "US", "language": "en-US", "platform": "Apple", "" } ] }' \
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

Push information related to the real-money purchases made by each user.

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

Push information related to in-game virtual currency purchases made by each user. Please note: If an item is bought by first converting real money to in-game money, it should appear in both purchases and virtual purchases.

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

> Endpoint URL. Replace **:game_id** and **:api_key** with your own Game ID and API Key.

```shell
https://api.12traits.com/v1/games/playfab?api_key=:api_key
```

### How to configure a PlayFab webhook

1. Go to PlayFab Analytics
2. Open "Webhooks" tab
3. Click "New Webhook"
4. Give it a name
5. Set Endpoint URL

# Push AppStart Events
If you are using Unity, you can push AppStart events using this endpoint. AppStart events are Dispatched at the start of a new app session.
> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "events": [ { "ts": 1501643068378, "appid": "1001", "type": "appStart", "userid": "901", "sessionid": "3213218932893", "platform": "iPhone", "sdk_ver": "1.2.3", "debug_device": false, "user_agent": "CFNetwork/808.1.4", "submit_time": 1501643088378 } ] }' \
https://api.12traits.com/v1/games/unity/app-start-events
```

### AppStartEvent
**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
ts|long|Yes|The timestamp (in milliseconds) at which the event was generated on the device.
appid|string|Yes|The ID that is assigned to each app on the Unity Analytics Dashboard
type|string|Yes|The type of event being queried (i.e. Custom, DeviceInfo, Transaction, etc)
userid|string|Yes|Unity Analytics generated identifier for a userid
sessionid|string|Yes|Unity Analytics generated identifier for the session.
platform|string|Yes|The platform that the session was played on
sdk_ver|string|Yes|Version of Unity Analytics SDK that is being used for this event.
debug_device|string|Yes|A boolean that shows whether the event was sent from a development build.
user_agent|string|Yes|The User-Agent request-header field
submit_time|long|Yes|The timestamp (in milliseconds) at which the event was received by Unity Analytics

# Push AppRunning Events
If you are using Unity, you can push AppRunning events using this endpoint. AppRunning events are dispatched periodically while an app runs.
> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "events": [ { "ts": 1501643068378, "appid": "1001", "type": "appRunning", "userid": "901", "sessionid": "3213218932893", "platform": "iPhone", "sdk_ver": "1.2.3", "debug_device": false, "user_agent": "CFNetwork/808.1.4", "submit_time": 1501643088378, "duration": 32832918 } ] }' \
https://api.12traits.com/v1/games/unity/app-running-events
```

### AppRunningEvent
**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
ts|long|Yes|The timestamp (in milliseconds) at which the event was generated on the device.
appid|string|Yes|The ID that is assigned to each app on the Unity Analytics Dashboard
type|string|Yes|The type of event being queried (i.e. Custom, DeviceInfo, Transaction, etc)
userid|string|Yes|Unity Analytics generated identifier for a userid
sessionid|string|Yes|Unity Analytics generated identifier for the session.
platform|string|Yes|The platform that the session was played on
sdk_ver|string|Yes|Version of Unity Analytics SDK that is being used for this event.
debug_device|string|Yes|A boolean that shows whether the event was sent from a development build.
user_agent|string|Yes|The User-Agent request-header field
submit_time|long|Yes|The timestamp (in milliseconds) at which the event was received by Unity Analytics
duration|long|Yes|The time it took to export data (in milliseconds).

# Push DeviceInfo Events
If you are using Unity, you can push DeviceInfo events using this endpoint. DeviceInfo events are dispatched the first time a user launches an app and whenever device information changes.
> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "events": [ { "ts": 1501643068378, "appid": "1001", "type": "deviceInfo", "userid": "901", "sessionid": "3213218932893", "platform": "iPhone", "sdk_ver": "1.2.3", "debug_device": false, "user_agent": "CFNetwork/808.1.4", "submit_time": 1501643088378, "debug_build": true, "rooted_jailbroken": false, "processor_type": "my_processor", "system_memory_size": "3902193", "make": "Apple", "app_ver": "1.3.4", "license_type": "advanced_pro", "app_instal_mode": "store", "model": "iPhone XS Max", "engine_ver": "1.3.2", "os_ver": "Mac OS X 10.11.5", "app_name": "com.Package.myGame", "timezone": "GMT+1", "ads_tracking": false } ] }' \
https://api.12traits.com/v1/games/unity/device-info-events
```

### DeviceInfoEvent
**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
ts|long|Yes|The timestamp (in milliseconds) at which the event was generated on the device.
appid|string|Yes|The ID that is assigned to each app on the Unity Analytics Dashboard
type|string|Yes|The type of event being queried (i.e. Custom, DeviceInfo, Transaction, etc)
userid|string|Yes|Unity Analytics generated identifier for a userid
sessionid|string|Yes|Unity Analytics generated identifier for the session.
platform|string|Yes|The platform that the session was played on
sdk_ver|string|Yes|Version of Unity Analytics SDK that is being used for this event.
debug_device|string|Yes|A boolean that shows whether the event was sent from a development build.
user_agent|string|Yes|The User-Agent request-header field
submit_time|long|Yes|The timestamp (in milliseconds) at which the event was received by Unity Analytics
debug_build|boolean|Yes|A boolean that shows whether the event was sent from a development build.
rooted_jailbroken|boolean|Yes|A boolean that is set to TRUE in case of rooted / jailbroken phone not sent for normal phone
processor_type|string|Yes|Device processor type
system_memory_size|string|Yes|Device system memory
make|string|Yes|Maker of the device
app_ver|string|Yes|Version of the application
license_type|string|Yes|Type of license
app_install_mode|string|Yes|Tells if app is installed via app store (“store”), adhoc (“adhoc”), developer install (“dev_release”), simulator (“simulator”) or enterprise (“enterprise”)
model|string|Yes|Device model
engine_ver|string|Yes|Unity engine version
os_ver|string|Yes|OS Version
app_name|string|Yes|Bundle identifier or package name
timezone|string|Yes|ISO code
ads_tracking|boolean|Yes|A boolean value that indicates whether the user has limited ad tracking

# Push Transaction Events
If you are using Unity, you can push Transaction events using this endpoint. Transaction events are for tracking monetization transactions through in-app purchases.

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "events": [ { "ts": 1501643068378, "appid": "1001", "type": "transaction", "userid": "901", "sessionid": "3213218932893", "platform": "iPhone", "sdk_ver": "1.2.3", "debug_device": false, "user_agent": "CFNetwork/808.1.4", "submit_time": 1501643088378, "receipt": { "data": "", "signature": "a4fe9582f23dea98cf" }, "currency": "EUR", "amount": 10.0, "transactionid": 9302931, "productid": "app03" } ] }' \
https://api.12traits.com/v1/games/unity/transaction-events
```

### TransactionEvent
**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
ts|long|Yes|The timestamp (in milliseconds) at which the event was generated on the device.
appid|string|Yes|The ID that is assigned to each app on the Unity Analytics Dashboard
type|string|Yes|The type of event being queried (i.e. Custom, DeviceInfo, Transaction, etc)
userid|string|Yes|Unity Analytics generated identifier for a userid
sessionid|string|Yes|Unity Analytics generated identifier for the session.
platform|string|Yes|The platform that the session was played on
sdk_ver|string|Yes|Version of Unity Analytics SDK that is being used for this event.
debug_device|string|Yes|A boolean that shows whether the event was sent from a development build.
user_agent|string|Yes|The User-Agent request-header field
submit_time|long|Yes|The timestamp (in milliseconds) at which the event was received by Unity Analytics
receipt|ReceiptRecord|No|Data returned by platform app stores, such as App Store and Google Play
currency|string|Yes|The currency code for the payment (eg. USD, EUR, CAD, etc) in ISO 4217 code
amount|float|Yes|The total decimal amount of currency spent
transactionid|int|Yes|Unique identifier for this transaction, set by SDK. 
productid|string|Yes|The store-specific product identifier of an in-app purchase

### ReceiptRecord
**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
Data|string|Yes|Data received from app store
Signature|string|Yes|Receipt signature

# Push Custom Events
If you are using Unity, you can push Custom events using this endpoint. Custom events are any other kind of events your game uses that are not in the above categories.

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "events": [ { "ts": 1501643068378, "appid": "1001", "type": "custom", "userid": "901", "sessionid": "3213218932893", "platform": "iPhone", "sdk_ver": "1.2.3", "debug_device": false, "user_agent": "CFNetwork/808.1.4", "submit_time": 1501643088378, "name": "my_custom_event" } ] }' \
https://api.12traits.com/v1/games/unity/custom-events
```

### CustomEvent
**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
ts|long|Yes|The timestamp (in milliseconds) at which the event was generated on the device.
appid|string|Yes|The ID that is assigned to each app on the Unity Analytics Dashboard
type|string|Yes|The type of event being queried (i.e. Custom, DeviceInfo, Transaction, etc)
userid|string|Yes|Unity Analytics generated identifier for a userid
sessionid|string|Yes|Unity Analytics generated identifier for the session.
platform|string|Yes|The platform that the session was played on
sdk_ver|string|Yes|Version of Unity Analytics SDK that is being used for this event.
debug_device|string|Yes|A boolean that shows whether the event was sent from a development build.
user_agent|string|Yes|The User-Agent request-header field
submit_time|long|Yes|The timestamp (in milliseconds) at which the event was received by Unity Analytics
name|string|Yes|The custom event name
custom_params|map[paramName: paramValue]|No|The params sent for this custom event

