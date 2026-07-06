---
description: >-
  Send SUI coins to a list of addresses, following a list of amounts. This is
  for SUI coin only and does not require a separate gas coin object.
  Specifically, what pay_sui does are: 1. debit each input_.
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
| `gas_budget`  | BigInt\_for\_uint64 | Yes      | the gas budget; the transaction fails if the gas cost exceeds the budget       |

## Response

Returns `TransactionBlockBytes`.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
