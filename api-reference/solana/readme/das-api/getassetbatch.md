---
description: >-
  Retrieve detailed information about multiple digital assets by their IDs. An
  alias of getAssets.
---

# getAssetBatch

`getAssetBatch` is an alias of [getAssets](getassets.md). It returns metadata for a set of assets in one call, including the asset URL, metadata, collection, creators, authorities, compression status, and ownership.

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": "123",
  "method": "getAssetBatch",
  "params": {
    "ids": [
      "7k7YqWc85BtyTx5UyvNnogv2jPHVe4BZ9HQsFLM8VnoJ",
      "HP2QQgwfTgHNRcX7g3HHMNKxXjAEvQsr1wComqbPfWAR"
    ]
  }
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Name | Description                     |
| ---- | ------------------------------- |
| ids  | A set of asset IDs to retrieve. |
