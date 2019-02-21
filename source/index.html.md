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

# Authentication

> Send your API Key with the request.

```shell
curl -XPOST -H "Authorization: Bearer <API_KEY>" https://api.12traits.com
```

# Push game actions

> Push the player’s actions. It is possible to push up to 1000 multiple actions at the same time.

```shell
curl -XPOST \
-H "Authorization: Bearer <API_KEY>" \
-H "Content-Type: application/json" \
-d '{ "actions": [ {"timestamp": 1000000000, "player_id": "1001", "action_id": "2002", "action_name": "buy_item", "action_params": { "item_id": "3003" }} ] }'
https://api.12traits.com/v1/games/abc/actions
```

### Action Parameters

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
timestamp|Unix Timestamp|Yes|UTC
player_id|string|Yes|Unique player ID
action_id|string| |Action ID specific to the game
action_name|string| |User-friendly name, for example: "buy_item", "start_fight"
action_params|map[string]string| |Map of action params

### Response

```js
{
  "code": 201,
  "message": "50 action(s) have been successfully ingested",
  "data": null
}
```