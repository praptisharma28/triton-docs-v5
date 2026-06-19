---
description: >-
  Create an unsigned transaction to send SUI coin object to a Sui address. The
  SUI object is also used as the gas object.
---

# unsafe\_transferSui

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "unsafe_transferSui",
  "params": [
    "<signer>",
    "<sui_object_id>",
    "<gas_budget>",
    "<recipient>",
    "<amount>"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter       | Type                | Required | Description                                                                 |
| --------------- | ------------------- | -------- | --------------------------------------------------------------------------- |
| `signer`        | SuiAddress          | Yes      | the transaction signer's Sui address                                        |
| `sui_object_id` | ObjectID            | Yes      | the Sui coin object to be used in this transaction                          |
| `gas_budget`    | BigInt\_for\_uint64 | Yes      | the gas budget, the transaction will fail if the gas cost exceed the budget |
| `recipient`     | SuiAddress          | Yes      | the recipient's Sui address                                                 |
| `amount`        | BigInt\_for\_uint64 | No       | the amount to be split out and transferred                                  |

## Response

Returns `TransactionBlockBytes`.
