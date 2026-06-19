---
description: >-
  This method retrieves detailed information about a multiple digital
  assets/NFTs. The metadata returned includes information about the asset URL,
  metadata, collection, creators, authorities, compressio
---

# getAssets

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": "123",
  "method": "getAssets",
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
