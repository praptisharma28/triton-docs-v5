---
description: >-
  Returns the number of transactions in a block from a block matching the given
  block hash.
---

# eth\_getBlockTransactionCountByHash

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_getBlockTransactionCountByHash",
  "params": [
    "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter    | Type   | Required | Description |
| ------------ | ------ | -------- | ----------- |
| `Block hash` | hash32 | No       |             |

## Response

Returns `Transaction count`.

```json
"0x8"
```
