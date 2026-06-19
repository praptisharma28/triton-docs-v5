---
description: >-
  Send SUI coins to a list of addresses, following a list of amounts. This is
  for SUI coin only and does not require a separate gas coin object.
  Specifically, what pay_sui does are: 1. debit each input_
---

# unsafe\_paySui

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "unsafe_paySui",
  "params": [
    "<signer>",
    "<input_coins>",
    "<recipients>",
    "<amounts>",
    "<gas_budget>"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter     | Type                | Required | Description                                                                       |
| ------------- | ------------------- | -------- | --------------------------------------------------------------------------------- |
| `signer`      | SuiAddress          | Yes      | the transaction signer's Sui address                                              |
| `input_coins` | array               | Yes      | the Sui coins to be used in this transaction, including the coin for gas payment. |
| `recipients`  | array               | Yes      | the recipients' addresses, the length of this vector must be the same as amounts. |
| `amounts`     | array               | Yes      | the amounts to be transferred to recipients, following the same order             |
| `gas_budget`  | BigInt\_for\_uint64 | Yes      | the gas budget, the transaction will fail if the gas cost exceed the budget       |

## Response

Returns `TransactionBlockBytes`.
