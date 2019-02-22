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
-d '{ "actions": [ {"timestamp": 1000000000, "player_id": "1001", "action_id": "2002", "action_name": "buy_item", "action_params": [ {"key": "item_id", "value": "3003" } ] ] }'
https://api.12traits.com/v1/games/abc/actions
```

### Action

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
timestamp|Timestamp||YYYY-MM-DD HH:MM:SS
player_id|string|Yes|Unique player ID
action_id|string|Yes|Action ID specific to the game
action_name|string|Yes|User-friendly name, for example: "buy_item", "start_fight"
action_params|list<ActionParam>| |Map of action params

### ActionParam

**Field**|**Type**|**Required**|**Description**
-----|-----|-----|-----
key|string|Yes|Action param key
value|string|Yes|Action param value

### Response

```js
{
  "code": 201,
  "message": "50 action(s) have been successfully ingested",
  "data": null
}
```