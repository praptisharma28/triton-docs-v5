---
description: >-
  Returns information about a transaction by block hash and transaction index
  position.
---

# eth\_getTransactionByBlockHashAndIndex

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_getTransactionByBlockHashAndIndex",
  "params": [
    "0xbf137c3a7a1ebdfac21252765e5d7f40d115c2757e4a4abee929be88c624fdb7",
    "0x2"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter           | Type   | Required | Description |
| ------------------- | ------ | -------- | ----------- |
| `Block hash`        | hash32 | Yes      |             |
| `Transaction index` | uint   | Yes      |             |

## Response

Returns `Transaction information`.

```json
{
  "blockHash": "0x510efccf44a192e6e34bcb439a1947e24b86244280762cbb006858c237093fda",
  "blockNumber": "0x422",
  "chainId": "0x7e2",
  "from": "0xfe3b557e8fb62b89f4916b721be55ceb828dbd73",
  "gas": "0x5208",
  "gasPrice": "0x3b9aca00",
  "hash": "0xa52be92809541220ee0aaaede6047d9a6c5d0cd96a517c854d944ee70a0ebb44",
  "input": "0x",
  "nonce": "0x1",
  "to": "0x627306090abab3a6e1400e9345bc60c78a8bef57",
  "transactionIndex": "0x0",
  "value": "0x4e1003b28d9280000",
  "v": "0xfe7",
  "r": "0x84caf09aefbd5e539295acc67217563438a4efb224879b6855f56857fa2037d3",
  "s": "0x5e863be3829812c81439f0ae9d8ecb832b531d651fb234c848d1bf45e62be8b9"
}
```
