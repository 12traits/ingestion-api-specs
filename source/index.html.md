---
title: 12traits Ingestion API Reference

language_tabs:
  - go

toc_footers:
  - <a href='https://12traits.com'>Sign Up to get API Key</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the 12traits Ingestion API!

# Authentication

> Send your API Key with the request:

```shell
curl -XPOST \
-H “Authorization: Bearer <API_KEY>” \
-H "Content-Type: application/json" \
-d { “actions”: [ {“timestamp”: 1000000000, “player_id”: “1001”} ] }
https://api.12traits.com/v1/abc/actions
```