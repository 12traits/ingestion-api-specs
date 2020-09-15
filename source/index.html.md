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

The 12traits API uses API Keys to authenticate requests. You can view and manage your API Keys in the 12traits Dashboard. Each dashboard has its own API Key.

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

> Send your API Key with the request.

```shell
curl -XPOST -H "Authorization: Bearer <API_KEY>" https://api.12traits.com/v1/...
```

# API limits

- It is possible to push up to 1000 multiple items at the same time.

# Postman Collection

<a href="https://www.getpostman.com/" target="_blank">Postman</a> is helpful when first integrating with the 12traits API. Postman is a powerful tool that will help you call and debug specific response data, as well as any error messages.

## How to use 12traits API Postman Collection

1. Go to <a href="https://documenter.getpostman.com/view/6074213/Szzkdxfm">https://documenter.getpostman.com/view/6074213/Szzkdxfm</a>
2. Click "Run in Postman"
3. Set your environment variables in Postman App

### 12traits API variables

- **BASE_URL** - Production - https://api.12traits.com, or sandbox - https://sandbox.api.12traits.com
- **API_KEY** - Paste your own API Key here

Now you are ready to test 12traits API endpoints from Postman!

# Push Custom Survey Data

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: multipart/form-data" \
-F file=@2019-09-23.csv \
https://api.12traits.com/v1/survey-data/csv
```

> Response

```json
{
    "code": 201,
    "message": "ingestion has been started",
    "data": {
        "estimated_time_to_complete": 125
    }
}
```

You can send any custom data from your own surveys by uploading a CSV file in the [following format](https://storage.googleapis.com/12traits/12traits_custom_data_template.csv).

# Push KPIs as CSV

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: multipart/form-data" \
-F file=@2019-09-23.csv \
https://api.12traits.com/v1/kpis/csv
```

> Response

```json
{
    "code": 201,
    "message": "ingestion has been started",
    "data": {
        "estimated_time_to_complete": 125
    }
}
```

You can combine daily KPIs for all users in a single CSV file and send it at once. The CSV file should follow the [following format](https://storage.googleapis.com/12traits/kpis_template.csv).

# Push User Actions

Push user actions. This can be any set of actions that your app generates.

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "actions": [ { "timestamp": "2019-01-01 15:04:05", "user_id": "1001", "action_id": "2002", "action_name": "buy_item", "action_params": [ {"key": "item_id", "value": "3003" } ] } ] }' \
https://api.12traits.com/v1/actions
```

### Action

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
timestamp|Timestamp||YYYY-MM-DD HH:MM:SS in UTC
user_id|string|Yes|Unique user ID
action_id|string|Yes|Action ID specific to your app
action_name|string|Yes|User-friendly name, for example: "buy_item", "start_fight"
action_params|list\<ActionParam\>| |Map of action params

### ActionParam

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
key|string|Yes|Action param key
value|string|Yes|Action param value

> Example response

```json
{
  "code": 201,
  "message": "50 actions have been successfully ingested",
  "data": null
}
```

# Push Logins

Push information when users log into the app.

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "users": [ { "timestamp": "2019-01-01 15:04:05", "user_id": "1001", "clan_id": "2002", "level": 10, "device": "iPhone X", "country": "US", "language": "en-US", "platform": "Apple", "register_date": "2018-11-14", "last_login_timestamp": "2019-01-01 11:10:06" } ] }' \
https://api.12traits.com/v1/logins
```

### User

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
timestamp|Timestamp||YYYY-MM-DD HH:MM:SS in UTC
user_id|string|Yes|Unique user ID
clan_id|string||Clan/guild/organization the user belongs to
level|int||User's level or rank in the app
device|string||Device of the user. e.g. iPad, iPhone X
country|string||Country from where user has logged in in ISO Alpha-2 format (2 letters)
platform|string||Platform user is using, e.g. Steam
language|string||User's language in [IETF format](https://en.wikipedia.org/wiki/IETF_language_tag)
register_date|string||YYYY-MM-DD
last_login_timestamp|Timestamp||YYYY-MM-DD HH:MM:SS in UTC

> Example response

```json
{
  "code": 201,
  "message": "50 users have been successfully ingested",
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
-d '{ "purchases": [ { "timestamp": "2019-01-01 15:04:05", "user_id": "1001", "item_id": "2002", "count": 1, "price": 25.50, "platform": "Steam" } ] }' \
https://api.12traits.com/v1/purchases
```

### Purchase

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
timestamp|Timestamp||YYYY-MM-DD HH:MM:SS in UTC
user_id|string|Yes|Unique user ID
item_id|string|Yes|In app Item ID
count|int|Yes|Number of items purchased
price|float|Yes|Total money spent on the purchase
platform|string||Platform user is using, e.g. Steam

> Example response

```json
{
  "code": 201,
  "message": "50 purchases have been successfully ingested",
  "data": null
}
```

# Push Virtual Purchases

Push information related to in-app virtual currency purchases made by each user.

Please note: If an item is bought by first converting real money to in-app money, it should appear in both purchases and virtual purchases.

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "virtual_purchases": [ { "timestamp": "2019-01-01 15:04:05", "user_id": "1001", "item_id": "2002", "count": 1, "virtual_price": 25.50, "currency": "gold" } ] }' \
https://api.12traits.com/v1/virtual-purchases
```

### VirtualPurchase

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
timestamp|Timestamp||YYYY-MM-DD HH:MM:SS in UTC
user_id|string|Yes|Unique user ID
item_id|string|Yes|In-app Item ID
count|int|Yes|Number of items purchased
virtual_price|float|Yes|Total virtual currency spent on the purchase
currency|string|Yes|Currency of the purchase, e.g. "gold","tokens","crowns"

> Example response

```json
{
  "code": 201,
  "message": "50 virtual purchases have been successfully ingested",
  "data": null
}
```

# Push Level Ups

Push level-ups for each user. Levels refer to app-content levels, i.e. levels that represent the progress of a user through the app. These can have different names (tiers/stages/progress/ranks, etc.), these should always be increasing.

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "level_ups": [ { "timestamp": "2019-01-01 15:04:05", "user_id": "1001", "level": 10 } ] }' \
https://api.12traits.com/v1/level-ups
```

### LevelUp

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
timestamp|Timestamp||YYYY-MM-DD HH:MM:SS in UTC
user_id|string|Yes|Unique user ID
level|int|Yes|New level after level-up

> Example response

```json
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
https://api.12traits.com/v1/items
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

```json
{
  "code": 201,
  "message": "50 items have been successfully ingested",
  "data": null
}
```

# Push Social Dynamics

Push information on the social dynamics of the user, i.e. interactions between users.

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "social_dynamics": [ { "timestamp": "2019-01-01 15:04:05", "user_id": "1001", "target_user_id": "1002", "connection": "linked", "interaction": "friend" } ] }' \
https://api.12traits.com/v1/social-dynamics
```

### SocialDynamics

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
timestamp|Timestamp||YYYY-MM-DD HH:MM:SS in UTC
user_id|string|Yes|Unique user ID
target_user_id|string|Yes|ID of the user with whom a link is made or who is unlinked
connection|string|Yes|"linked" or "unllinked"
interaction|string|Yes|"friend" or "party"

> Example response

```json
{
  "code": 201,
  "message": "50 social dynamics have been successfully ingested",
  "data": null
}
```

# PlayFab Webhook Integration

If you are using PlayFab you are able to integrate it with 12traits by using [PlayFab Webhooks](https://api.playfab.com/docs/tutorials/landing-analytics/webhooks). You can simply configure PlayFab to send all events to 12traits by using this endpoint.

> Endpoint URL. Replace **:api_key** with your own API Key.

```shell
https://api.12traits.com/v1/playfab?api_key=:api_key
```

### How to configure a PlayFab webhook

1. Go to PlayFab Analytics
2. Open "Webhooks" tab
3. Click "New Webhook"
4. Give it a name
5. Set Endpoint URL

# Get Assessment Response Status

> Request

```shell
curl -XGET \
-H "Authorization: Bearer <API_KEY>" \
https://api.12traits.com/v1/assessment/user/status?id=:userid
```

> Response

```json
{
    "code": 200,
    "message": "",
    "data": {
        "completed": true,
        "completed_at": 1585806949
    }
}
```

This endpoint allows you to check if user has completed survey or not.

<aside class="notice">
Replace <b>:userid</b> with exactly the same encrypted ID you used to send user to the survey.
</aside>

# Remove User Data

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "id": "$user_id" }' \
https://api.12traits.com/v1/user-data
```

Delete user data permanently. This endpoint can be used if a given user requested data deletion.

<aside class="notice">
Replace <b>$user_id</b> with exactly the same encrypted ID you used to send user to the survey or you used to send behavioural data for.
</aside>

# Get Segment / Persona User IDs

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
https://api.12traits.com/v1/segments/:segment_id/personas/:persona/users
```

> Response

```json
{
    "code": 200,
    "message": "",
    "data": {
        "ids": ["1001", "1002"]
    }
}
```

Get the list of Segment / Persona user IDs.

<aside class="notice">
Replace <b>:segment_id</b> with a valid ID of the segment which you can find on your dashboard. Use <b>default</b> for default segment.
</aside>

<aside class="notice">
Replace <b>:persona</b> with a persona index (starting with 1). Use <b>all</b> to fetch all personas.
</aside>

# Get Segment / Persona traits

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
https://api.12traits.com/v1/segments/:segment_id/personas/:persona/traits
```

> Response

```json
{
    "code": 200,
    "message": "",
    "data": {
        "agreeableness": {
            "Altruism": 64.83333587646484,
            "Modesty": 48.00000762939453,
            "Trust": 62.00000762939453
        },
        "communication": {
            "Feeler": 54.00000762939453,
            "Intuitor": 39,
            "Sensor": 62.66666793823242,
            "Thinker": 43.99999237060547
        },
        "competitiveness": {
            "Avoidant": 44.33332824707031,
            "Collaborative": 61.66666793823242,
            "Competitive affectivity": 37,
            "Dependent": 48.83332824707031,
            "Dominant": 43.499996185302734,
            "General competitiveness": 44.833343505859375,
            "Independent": 58.666656494140625,
            "Personal enhancement": 54.000003814697266
        },
        "conscientiousness": {
            "Achievement striving": 60.48037338256836,
            "Competence": 51.666664123535156,
            "Self-discipline": 66.1666488647461
        },
        "coreMotivations": {
            "Autonomy": 50.83333206176758,
            "Mastery": 67.1666488647461,
            "Purpose": 60.33332824707031
        },
        "culture": {
            "Individualism": 58.00001525878906,
            "Indulgence": 51.99998474121094,
            "Long term orientation": 58.66665267944336,
            "Masculinity": 45.666664123535156,
            "Power distance": 53.50000762939453,
            "Uncertainty avoidance": 57.33333969116211
        },
        "extraversion": {
            "Activity": 52.33333206176758,
            "Assertiveness": 52.499996185302734,
            "Warmth": 48.666664123535156
        },
        "motivations": {
            "Compensatory effort": 63.666664123535156,
            "Competitiveness": 37,
            "Confidence in success": 64.33332824707031,
            "Difficult tasks": 58.16666030883789,
            "Dominance": 59.50001525878906,
            "Eagerness to learn": 66.33333587646484,
            "Engagement": 55.16666030883789,
            "Fearlessness": 61.66666030883789,
            "Flexibility": 41.166656494140625,
            "Flow": 75.66666412353516,
            "Goal setting": 46.666664123535156,
            "Independence": 58.999996185302734,
            "Internality": 57.833335876464844,
            "Persistence": 70,
            "Pride in productivity": 77.99999237060547,
            "Self-control": 72.00001525878906,
            "Status orientation": 62.00000762939453
        },
        "neuroticism": {
            "Anxiety": 56.16666793823242,
            "Impulsiveness": 39.33333206176758,
            "Vulnerability": 38.5
        },
        "openness": {
            "Actions": 41.166656494140625,
            "Empathy": 62.16666793823242,
            "Fantasy": 56.999961853027344
        },
        "personality": {
            "Agreeableness": 58.499996185302734,
            "Conscientiousness": 59.000003814697266,
            "Extraversion": 51.83332443237305,
            "Neuroticism": 45.000003814697266,
            "Openness": 51
        },
        "wellBeing": {
            "Autonomy": 50.83333206176758,
            "Bounded reciprocity": 61,
            "Grit": 64.33332824707031,
            "Mastery": 67.1666488647461,
            "Physical activity level": 50.16667175292969,
            "Physical well-being": 45.16667175292969,
            "Psychological well-being": 56.16666793823242,
            "Purpose": 60.33332824707031,
            "Resilience": 62.83333969116211,
            "Sleep": 40.16666030883789,
            "Social well-being": 58.999996185302734
        }
    }
}
```

Get main aggregated Persona traits split by categories.

<aside class="notice">
Replace <b>:segment_id</b> with a valid ID of the segment which you can find on your dashboard. Use <b>default</b> for default segment.
</aside>

<aside class="notice">
Replace <b>:persona</b> with a persona index (starting with 1). Use <b>all</b> to fetch all personas.
</aside>

# Behavioural Data Integration

In order to support larger data volumes, 12traits provides support for cloud-based object storage technologies. These solutions are optimized, cost-effective, and secure ways of storing and sharing arbitrarily large datasets (Big Data).

## Supported cloud object storages

- [Google Cloud Storage](https://cloud.google.com/storage)
- [Amazon Web Services Simple Storage Service](https://aws.amazon.com/s3/)
- [Microsoft Azure Storage](https://azure.microsoft.com/en-us/services/storage/)

## Supported data formats

- [Newline delimited JSON](http://ndjson.org/) (Preferred by 12traits): The individual lines in the file (separated by `\n`) are valid JSON objects, but the complete file may not be
- Comma-separated values (CSV)
- Parquet

## Integration steps

### Configure access

Store your files separated by their category in the cloud object storage of your choice. Please include separate root directories for the various data types: `purchases`, `logins`, `actions`, `kpis`, `virtual_purchases`, `level_ups`, `items`, and `social_dynamics`. Directories can have any number of subfolders, for example `purchases/date=16000000000/load_id=fgdhb4gwv34t/purchases.csv` (Hive-partitioned directory structure). If using a flat directory structure (files right in the root directories), please include a timestamp or some other time-based UUID in the filenames. For example, `purchases/purchases_1600089751.csv`, `purchases/purchases_1600089777.csv`, etc.

<aside class="notice">
Ensure that 12traits can access the storage.
</aside>

#### AWS S3

- Set IAM policy for 12traits' AWS user (will be provided to you) enabling LIST and GET operations on the contents of the S3 bucket, or
- Share new credentials to the AWS S3 bucket storing the data files (access key and secret key)

#### Google Storage

- Set IAM policy for 12traits' GCP service account (will be provided to you) enabling LIST and GET operations on the contents of the GCS bucket, or
- Share service account key in JSON format

#### Microsoft Azure Storage

- Make sure that your storage account’s firewall settings [allow connectivity](https://docs.microsoft.com/en-us/azure/storage/common/storage-network-security#change-the-default-network-access-rule)
- Share a [Shared Access Token (SAS)](https://docs.microsoft.com/en-us/azure/storage/blobs/security-recommendations) with LIST and GET ACL on your files with 12traits

12traits will attempt to connect to your cloud storage, test connectivity, and validate file contents. We will send a notification in case of error:

- Unable to connect to the storage
- JSON/CSV format is not valid

If everything is okay, your data will be regularly processed by 12traits.

### Automating the integration

You can write any amount of data into your cloud object storage area of choice. New data files will be automatically identified and processed by 12traits. A frequent pattern of automation could be daily scheduled upload of data to the cloud storage. Please upload new files instead of overwriting existing ones.

We expect only new changes to the data to be shared (incremental load pattern) instead of pushing full dataset repeatedly.

## User Actions

Expected folder: `actions/`

Purpose: track arbitrary user actions. This data type provides a flexible structure to identify a wide variety of actions or activities that the user may take in the game. The differentiation between different actions (on top of setting their specific ID, name, and associated parameters) is completely up to you, but we advise you to keep the actions small, granular, and easy to associate with a specific player.

Please provide a real-time, event-based collection of the data instead of aggregates if possible.

> An action indicating a fight won by a player (with ID 1001) against an enemy player (with ID 5005).

```json
{
  "timestamp": "2019-01-01 15:04:05",
  "user_id": "1001",
  "action_id": "2002",
  "action_name": "fight_win",
  "action_params": [{ "key": "enemy_id", "value": "5005" }]
}

```

> An action indicating that a player (with ID 1001) chose to equip a new skin with ID 345344.

```json
{
  "timestamp": "2019-01-01 15:04:05",
  "user_id": "1001",
  "action_id": "4322",
  "action_name": "skin_equipped",
  "action_params": [{ "key": "skin_id", "value": "345344" }]
}
```

> An action indicating that the player with ID 1001 (fast-)traveled on the game’s map from Kingdom A (located on the map by the following 2D coordinates: X 12.3111 and Y 44.3111) to Kingdom B (X 33.4543 and Y 43.222).

```json
{
  "timestamp": "2019-01-01 15:04:05",
  "user_id": "1001",
  "action_id": "6765",
  "action_name": "fast_travel",
  "action_params": [
    { "key": "from_x", "value": "12.3111" },
    { "key": "from_y", "value": "44.3111" },
    { "key": "to_x", "value": "33.4543" },
    { "key": "to_y", "value": "43.222" },
    { "key": "from_location_id", "value": "kingdom_a" },
    { "key": "to_location_id", "value": "kingdom_b" }
  ]
}
```

> An action indicating that the user has used a certain item (ID: 34454).

```json
{
  "timestamp": "2019-01-01 15:04:05",
  "user_id": "1001",
  "action_id": "3222",
  "action_name": "item_used",
  "action_params": [{ "key": "item_id", "value": "34454" }]
}
```

> An action indicating that player has died.

```json
{
  "timestamp": "2019-01-01 15:04:05",
  "user_id": "1001",
  "action_id": "3222",
  "action_name": "user_died",
  "action_params": [{ "key": "died_reason", "value": "blunt-trauma" }]
}
```

> These example demonstrate multiple actions captured during a sample PvP fight:

> User 1001 equipped a medieval knight armour skin

```json
{
  "timestamp": "2019-01-01 15:04:05",
  "user_id": "1001",
  "action_id": "3222",
  "action_name": "skin_equipped",
  "action_params": [
    { "key": "skin_id", "value": "654" },
    { "key": "skin_name", "value": "medieval-knight-armor" }
  ]
}
```

> User 1001 used a health potion that restored 100 health points

```json
{
  "timestamp": "2019-01-01 15:11:11",
  "user_id": "1001",
  "action_id": "4001",
  "action_name": "item_used",
  "action_params": [
    { "key": "item_id", "value": "232" },
    { "key": "item_name", "value": "health-potion" },
    { "key": "potion_strength", "value": "100" }
  ]
}
```

> User 1001 started a fight against user 1033

```json
{
  "timestamp": "2019-01-01 15:12:30",
  "user_id": "1001",
  "action_id": "8000",
  "action_name": "fight_started",
  "action_params": [
    { "key": "enemy_id", "value": "1033" },
    { "key": "enemy_health", "value": "20000" },
    { "key": "fight_id", "value": "456354345" }
  ]
}
```

> User 1033 used a weapon against user 1001

```json
{
  "timestamp": "2019-01-01 15:12:33",
  "user_id": "1033",
  "action_id": "8001",
  "action_name": "use_weapon",
  "action_params": [
    { "key": "enemy_id", "value": "1001" },
    { "key": "damage", "value": "200" }
  ]
}
```

> User 1001 suffered a large damage and died

```json
{
  "timestamp": "2019-01-01 15:12:35",
  "user_id": "1001",
  "action_id": "6661",
  "action_name": "user_died",
  "action_params": [
    { "key": "died_reason", "value": "blunt-trauma" },
    { "key": "killed_by_user", "value": "1033" }
  ]
}
```

> User 1001 lost the fight

```json
{
  "timestamp": "2019-01-01 15:12:35",
  "user_id": "1001",
  "action_id": "8005",
  "action_name": "fight_lost",
  "action_params": [
    { "key": "enemy_id", "value": "1033" },
    { "key": "enemy_health", "value": "20000" },
    { "key": "fight_id", "value": "456354345" }
  ]
}
```

> User 1033 won the fight

```json
{
  "timestamp": "2019-01-01 15:12:35",
  "user_id": "1033",
  "action_id": "8006",
  "action_name": "fight_won",
  "action_params": [
    { "key": "enemy_id", "value": "1001" },
    { "key": "enemy_health", "value": "0" },
    { "key": "fight_id", "value": "456354345" }
  ]
}
```

## Logins

Expected folder: `logins/`

Purpose: track user log ins and information about users.

> User logged in:

```json
{
  "timestamp": "2019-01-01 15:04:05",
  "user_id": "1001",
  "clan_id": "2002",
  "level": 10,
  "device": "iPhone X",
  "country": "US",
  "language": "en-US",
  "platform": "Apple",
  "register_date": "2018-11-14",
  "last_login_timestamp": "2019-01-01 11:10:06"
}
```

## Purchases

Expected folder: `purchases/`

Track real-money purchases. Please provide a real-time, event-based collection of the data instead of aggregates if possible.

> User made in-app purchase:

```json
{
  "timestamp": "2019-01-01 15:04:05",
  "user_id": "1001",
  "item_id": "2002",
  "count": 1,
  "price": 25.5,
  "platform": "Steam"
}
```

## Virtual Purchases

Expected folder: `virtual_purchases/`

Track virtual purchases. Please provide a real-time, event-based collection of the data instead of aggregates if possible.

> User made in-app virtual purchase:

```json
{
  "timestamp": "2019-01-01 15:04:05",
  "user_id": "1001",
  "item_id": "2002",
  "count": 1,
  "virtual_price": 25.5,
  "currency": "gold"
}
```

## Level Ups

Expected folder: `level_ups/`

Purpose: track level-ups for each user. Please provide a real-time, event-based collection of the data instead of aggregates if possible.

> Initial user level:

```json
{ "timestamp": "2019-01-01 15:04:05", "user_id": "1001", "level": 10 }
```

## Items

Expected folder: `items/`

Purpose: track detailed information about each item.

> Weapon example:

```json
{
  "item_id": "1001",
  "price": 25.5,
  "currency": "USD",
  "name": "Axe",
  "type": "weapon"
}
```

## Social Dynamics

Expected folder: `social_dynamics/`

Purpose: track information on the social dynamics of the user, i.e. interactions between users. Please provide a real-time, event-based collection of the data instead of aggregates if possible.

> User sent a message to another user:

```json
{
  "timestamp": "2019-01-01 15:04:05",
  "user_id": "1001",
  "target_user_id": "1002",
  "interaction": "message_sent",
  "interaction_params": [
    { "key": "message_body", "value": "Hey! How are you? Long time no see!" }
  ]
}
```

> User befriended another user:

```json
{
  "timestamp": "2019-01-01 15:04:05",
  "user_id": "1001",
  "target_user_id": "1002",
  "interaction": "added_as_friend",
  "interaction_params": []
}
```

> User removed another user from their friends list:

```json
{
  "timestamp": "2019-01-01 15:04:05",
  "user_id": "1001",
  "target_user_id": "1002",
  "interaction": "unfriend",
  "interaction_params": []
}
```

> User gifted an item to another user:

```json
{
  "timestamp": "2019-01-01 15:04:05",
  "user_id": "1001",
  "target_user_id": "1002",
  "interaction": "gift_sent",
  "interaction_params": [
    { "key": "item_id", "value": "54645" },
    { "key": "message", "value": "Thanks for helping out the another day!" }
  ]
}
```

## KPIs

Expected folder: `kpis/`

Purpose: track detailed KPI data associated with a particular player. Any time granularity is supported, but we encourage at least daily KPIs.

> KPIs example

```json
{
  "timestamp": "2020-03-12 00:00:00",
  "user_id": "1001",
  "register_date": "2019-11-18",
  "churn_date": "",
  "kpi_id": "transaction_count",
  "value": 3.0
}
```
 