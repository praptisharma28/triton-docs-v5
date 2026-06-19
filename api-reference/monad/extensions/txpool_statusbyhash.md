---
description: Return the transaction-pool status for a given transaction hash.
---

# txpool\_statusByHash

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "txpool_statusByHash",
  "params": [
    "0xdc0818cf78f21a8e70579cb46a43643f78291264dda342ae31049421c82d21ae"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type   | Required | Description                             |
| --------- | ------ | -------- | --------------------------------------- |
| `hash`    | string | Yes      | 32-byte transaction hash (hex-encoded). |

## Response

```json
"0x..."
```
