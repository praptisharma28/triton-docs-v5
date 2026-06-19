---
description: >-
  Create an unsigned transaction to split a coin object into multiple equal-size
  coins.
---

# unsafe\_splitCoinEqual

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "unsafe_splitCoinEqual",
  "params": [
    "<signer>",
    "<coin_object_id>",
    "<split_count>",
    "<gas>",
    "<gas_budget>"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter        | Type                | Required | Description                                                                                                |
| ---------------- | ------------------- | -------- | ---------------------------------------------------------------------------------------------------------- |
| `signer`         | SuiAddress          | Yes      | the transaction signer's Sui address                                                                       |
| `coin_object_id` | ObjectID            | Yes      | the coin object to be split                                                                                |
| `split_count`    | BigInt\_for\_uint64 | Yes      | the number of coins to split into                                                                          |
| `gas`            | ObjectID            | No       | gas object to be used in this transaction, node will pick one from the signer's possession if not provided |
| `gas_budget`     | BigInt\_for\_uint64 | Yes      | the gas budget, the transaction will fail if the gas cost exceed the budget                                |

## Response

Returns `TransactionBlockBytes`.
