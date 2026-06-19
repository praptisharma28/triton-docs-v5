---
description: >-
  Fill in the defaults (gas, nonce, fees) for a transaction call object and
  return the unsigned transaction.
---

# eth\_fillTransaction

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_fillTransaction",
  "params": [
    {
      "to": "0x...",
      "data": "0x..."
    }
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type   | Required | Description              |
| --------- | ------ | -------- | ------------------------ |
| `tx`      | object | Yes      | Transaction call object. |

## Response

```json
{
  "raw": "0x",
  "tx": {
    "from": null,
    "to": null,
    "gas": null,
    "maxFeePerGas": null,
    "maxPriorityFeePerGas": null,
    "value": null,
    "input": null,
    "data": null,
    "nonce": null,
    "chainId": null,
    "accessList": null,
    "authorizationList": null
  }
}
```
