---
description: >-
  Send all SUI coins to one recipient. This is for SUI coin only and does not
  require a separate gas coin object. Specifically, what pay_all_sui does are:
  1. accumulate all SUI from input coins and depo
---

# unsafe\_payAllSui

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "unsafe_payAllSui",
  "params": [
    "<signer>",
    "<input_coins>",
    "<recipient>",
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
| `recipient`   | SuiAddress          | Yes      | the recipient address,                                                            |
| `gas_budget`  | BigInt\_for\_uint64 | Yes      | the gas budget, the transaction will fail if the gas cost exceed the budget       |

## Response

Returns `TransactionBlockBytes`.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
