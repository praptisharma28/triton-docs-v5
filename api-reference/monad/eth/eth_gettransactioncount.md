---
description: >-
  Returns the nonce of an account in the state. NOTE: The name
  eth_getTransactionCount reflects the historical fact that an account's nonce
  and sent transaction count were the same. After the Pectra for
---

# eth\_getTransactionCount

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_getTransactionCount",
  "params": [
    "0xc94770007dda54cF92009BFF0dE90c06F603a09f",
    "latest"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type                   | Required | Description |
| --------- | ---------------------- | -------- | ----------- |
| `Address` | address                | Yes      |             |
| `Block`   | BlockNumberOrTagOrHash | Yes      |             |

## Response

Returns `Account nonce`.

```json
"0x1"
```
