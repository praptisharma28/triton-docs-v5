---
description: Return the transaction-pool status for a given account address.
---

# txpool\_statusByAddress

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "txpool_statusByAddress",
  "params": [
    "0x407d73d8a49eeb85d32cf465507dd71d507100c1"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type   | Required | Description                            |
| --------- | ------ | -------- | -------------------------------------- |
| `address` | string | Yes      | 20-byte account address (hex-encoded). |

## Response

```json
"0x..."
```
