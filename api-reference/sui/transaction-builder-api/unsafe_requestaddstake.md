---
description: Add stake to a validator's staking pool using multiple coins and amount.
---

# unsafe\_requestAddStake

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "unsafe_requestAddStake",
  "params": [
    "<signer>",
    "<coins>",
    "<amount>",
    "<validator>",
    "<gas>",
    "<gas_budget>"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter    | Type                | Required | Description                                                                                                |
| ------------ | ------------------- | -------- | ---------------------------------------------------------------------------------------------------------- |
| `signer`     | SuiAddress          | Yes      | the transaction signer's Sui address                                                                       |
| `coins`      | array               | Yes      | Coin\&lt;SUI> object to stake                                                                              |
| `amount`     | BigInt\_for\_uint64 | No       | stake amount                                                                                               |
| `validator`  | SuiAddress          | Yes      | the validator's Sui address                                                                                |
| `gas`        | ObjectID            | No       | gas object to be used in this transaction, node will pick one from the signer's possession if not provided |
| `gas_budget` | BigInt\_for\_uint64 | Yes      | the gas budget, the transaction will fail if the gas cost exceed the budget                                |

## Response

Returns `TransactionBlockBytes`.
