---
description: >-
  Create an unsigned transaction to transfer an object from one address to
  another. The object's type must allow public transfers
---

# unsafe\_transferObject

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "unsafe_transferObject",
  "params": [
    "<signer>",
    "<object_id>",
    "<gas>",
    "<gas_budget>",
    "<recipient>"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter    | Type                | Required | Description                                                                                                |
| ------------ | ------------------- | -------- | ---------------------------------------------------------------------------------------------------------- |
| `signer`     | SuiAddress          | Yes      | the transaction signer's Sui address                                                                       |
| `object_id`  | ObjectID            | Yes      | the ID of the object to be transferred                                                                     |
| `gas`        | ObjectID            | No       | gas object to be used in this transaction, node will pick one from the signer's possession if not provided |
| `gas_budget` | BigInt\_for\_uint64 | Yes      | the gas budget, the transaction will fail if the gas cost exceed the budget                                |
| `recipient`  | SuiAddress          | Yes      | the recipient's Sui address                                                                                |

## Response

Returns `TransactionBlockBytes`.
