---
description: >-
  This method retrieves detailed information about a specific digital asset/NFT.
  The metadata returned includes information about the asset URL, metadata,
  collection, creators, authorities, compression
---

# getAsset

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": "123",
    "method": "getAsset",
    "params": {
      "id": "7k7YqWc85BtyTx5UyvNnogv2jPHVe4BZ9HQsFLM8VnoJ"
    }
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Name | Description                                     | Required |
| ---- | ----------------------------------------------- | -------- |
| id   | The unique identifier of the asset to retrieve. | Yes      |
