---
title: 12traits API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='https://12traits.com'>Sign Up to get API Key</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the 12traits API!

The 12traits API is a [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) API, which accepts and returns JSON-encoded data, and uses standard HTTP response codes, authentication, and verbs.

# Authentication

The 12traits API uses API Keys to authenticate requests. You can view and manage your API Keys in the 12traits Dashboard. Each dashboard has its own API Key.

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

> Send your API Key with the request.

```shell
curl -XPOST -H "Authorization: Bearer <API_KEY>" https://api.12traits.com/v1/...
```
# Register User for Survey

> Request

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "id": "$user_id" }' \
https://api.12traits.com/v1/assessment/user/register
```

> Response

```json
{
    "code": 200,
    "message": "",
    "data": {
        "expires_at": 1615369460,
        "token": "df430aa33c765e5bff12aa646560fa764310317a",
        "status": "requested"
    }
}
```

This endpoint let you register users who can access the survey, the token provided should be used to pass as `?playerid=` into the survey link. 

# Get Assessment Users List

> Request

```shell
curl -XGET \
-H "Authorization: Bearer <API_KEY>" \
https://api.12traits.com/v1/assessment/users?page=1
```

> Response

```json
{
    "code": 200,
    "message": "",
    "data": {
        "current_page": 1,
        "total_pages": 8,
        "users": [
            {
                "id": ":user_id1",
                "completed_at": "2021-02-25 03:51:11 EST"
            },
            {
                "id": ":user_id2",
                "completed_at": "2021-03-02 14:05:22 EST"
            },
            {
                "id": ":user_id3",
                "completed_at": "2021-03-02 15:44:17 EST"
            },
            {
                "id": ":user_id4",
                "completed_at": "2021-03-02 22:51:59 EST"
            },
            {
                "id": ":user_id5",
                "completed_at": "2021-03-03 00:50:16 EST"
            }
        ]
    }
}
```

Here you can get the list of users that completed the survey. You can use a ?page=x parameter, and each page contains maximum 500 users. This endpoint has a cache of one hour. 

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

# Get Survey Details

> Request

```shell
curl -XGET \
-H "Authorization: Bearer <API_KEY>" \
https://api.12traits.com/v1/assessment
```

> Response

```json
{
    "code": 200,
    "message": "survey details",
    "data": {
        "result_ok": true,
        "data": {
            "status": "Launched",
            "created_on": "2021-03-05 04:43:56",
            "modified_on": "2021-03-08 15:44:54",
            "completed_at": 0,
            "type": "Standard Survey",
            "title": "12traits Survey",
            "internal_title": "12traits Survey",
            "statistics": {
                "Partial": 2,
                "Disqualified": 1,
                "Complete": 7
            }
        }
    }
}
```

This endpoint allows you to check the Survey status, it can be either "Launched", "Closed", "Deleted".


# Remove User Data

> Request

```shell
curl -XDELETE \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "id": "$user_id" }' \
https://api.12traits.com/v1/userData
```

Delete user data permanently. This endpoint can be used if a given user requested data deletion.

<aside class="notice">
Replace <b>$user_id</b> with exactly the same ID you used to send user to the survey or you used to send behavioural data for.
</aside>

# Get Segment / Persona User IDs

> Request

```shell
curl -XGET \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
https://api.12traits.com/v1/segments/:segment_id/personas/:persona/user-ids
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
Replace <b>:segment_id</b> with a valid ID of the segment which you can find in your dashboard. Use <b>default</b> for default segment.
</aside>

<aside class="notice">
Replace <b>:persona</b> with a persona index (starting with 1). Use <b>all</b> to fetch all personas.
</aside>

# Get Segment / Persona Traits

> Request

```shell
curl -XGET \
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
        "category1": {
          "trait1": 62.34,
          "trait2": 13.15
        },
        "category2": {
          "trait3": 12.45,
          "trait4": 78.98
        }
    }
}
```

Get main aggregated Persona traits split by category.

<aside class="notice">
Replace <b>:segment_id</b> with a valid ID of the segment which you can find in your dashboard. Use <b>default</b> for default segment.
</aside>

<aside class="notice">
Replace <b>:persona</b> with a persona index (starting with 1). Use <b>all</b> to fetch all personas.
</aside>

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

# KPIs/Behavioural Data Integration

In order to support large data volumes, 12traits provides support for cloud-based object storage technologies. These solutions are optimized, cost-effective, and secure ways of storing and sharing arbitrarily large datasets (Big Data).

## Supported cloud object storages

- [Google Cloud Storage](https://cloud.google.com/storage)
- [Amazon Web Services Simple Storage Service](https://aws.amazon.com/s3/)
- [Microsoft Azure Storage](https://azure.microsoft.com/en-us/services/storage/)

## Supported data formats

- [Newline delimited JSON](http://ndjson.org/) (Preferred by 12traits): The individual lines in the file (separated by `\n`) are valid JSON objects, but the complete file may not be
- Comma-separated values (CSV)

## Integration steps

### Configure access

Store your files separated by their category in the cloud object storage of your choice.

Bucket structure:

- Please include separate root directories for the various data types: `purchases`, `logins`, `actions`, `kpis`, `virtual_purchases`, `level_ups`, `items`, and `social_dynamics`.
- Directories can have any number of subfolders, for example `purchases/date=16000000000/load_id=fgdhb4gwv34t/purchases.csv` (Hive-partitioned directory structure).
- If using a flat directory structure (files right in the root directories), please include a timestamp or some other time-based UUID in the filenames. For example, `purchases/purchases_1600089751.csv`, `purchases/purchases_1600089777.csv`, etc.
- Please indicate the file format for each filename:
  - Files with the prefix `.json` are assumed to be newline-delimited JSON files
  - Files with the prefix `.csv` are assumed to be CSV files

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

Purpose: track arbitrary user actions. This data type provides a flexible structure to identify a wide variety of actions or activities that the user may take in the game. The differentiation between different actions (on top of setting their specific ID, name, and associated parameters) is completely up to you, but we advise you to keep the actions small, granular, and easy to associate with a specific user.

Please provide a real-time, event-based collection of the data instead of aggregates if possible.

> An action indicating a fight won by a user (with ID 1001) against an enemy user (with ID 5005).

```json
{
  "timestamp": "2019-01-01 15:04:05",
  "user_id": "1001",
  "action_id": "2002",
  "action_name": "fight_win",
  "action_params": [{ "key": "enemy_id", "value": "5005" }]
}

```

> An action indicating that a user (with ID 1001) chose to equip a new skin with ID 345344.

```json
{
  "timestamp": "2019-01-01 15:04:05",
  "user_id": "1001",
  "action_id": "4322",
  "action_name": "skin_equipped",
  "action_params": [{ "key": "skin_id", "value": "345344" }]
}
```

> An action indicating that the user with ID 1001 (fast-)traveled on the game’s map from Kingdom A (located on the map by the following 2D coordinates: X 12.3111 and Y 44.3111) to Kingdom B (X 33.4543 and Y 43.222).

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

> An action indicating that user has died.

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

Purpose: track detailed KPI data associated with a particular user. Any time granularity is supported, but we encourage at least daily KPIs.

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
 