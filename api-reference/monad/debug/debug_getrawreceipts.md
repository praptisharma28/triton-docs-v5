---
description: Return the raw, RLP-encoded receipts for a block number or tag.
---

# debug\_getRawReceipts

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "debug_getRawReceipts",
  "params": [
    "latest"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type   | Required | Description                                                          |
| --------- | ------ | -------- | -------------------------------------------------------------------- |
| `block`   | string | Yes      | Block number (hex) or tag: `latest`, `safe`, `finalized`, `pending`. |

## Response

```json
[
  "0x..."
]
```
