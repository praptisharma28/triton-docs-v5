---
description: Return the raw, RLP-encoded transaction for a transaction hash.
---

# debug\_getRawTransaction

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "debug_getRawTransaction",
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
| `tx_hash` | string | Yes      | 32-byte transaction hash (hex-encoded). |

## Response

```json
"0x..."
```
