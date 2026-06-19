---
description: >-
  Send `Coin<T>` to a list of addresses, where `T` can be any coin type,
  following a list of amounts, The object specified in the `gas` field will be
  used to pay the gas fee for the transaction. The gas
---

# unsafe\_pay

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "unsafe_pay",
  "params": [
    "<signer>",
    "<input_coins>",
    "<recipients>",
    "<amounts>",
    "<gas>",
    "<gas_budget>"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter     | Type                | Required | Description                                                                                                |
| ------------- | ------------------- | -------- | ---------------------------------------------------------------------------------------------------------- |
| `signer`      | SuiAddress          | Yes      | the transaction signer's Sui address                                                                       |
| `input_coins` | array               | Yes      | the Sui coins to be used in this transaction                                                               |
| `recipients`  | array               | Yes      | the recipients' addresses, the length of this vector must be the same as amounts.                          |
| `amounts`     | array               | Yes      | the amounts to be transferred to recipients, following the same order                                      |
| `gas`         | ObjectID            | No       | gas object to be used in this transaction, node will pick one from the signer's possession if not provided |
| `gas_budget`  | BigInt\_for\_uint64 | Yes      | the gas budget, the transaction will fail if the gas cost exceed the budget                                |

## Response

Returns `TransactionBlockBytes`.
