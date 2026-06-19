---
description: Returns the number of transactions in a block matching the given block number.
---

# eth\_getBlockTransactionCountByNumber

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_getBlockTransactionCountByNumber",
  "params": [
    "0xe8"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type             | Required | Description |
| --------- | ---------------- | -------- | ----------- |
| `Block`   | BlockNumberOrTag | No       |             |

## Response

Returns `Transaction count`.

```json
"0x8"
```
