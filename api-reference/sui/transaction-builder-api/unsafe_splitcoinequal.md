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
| `gas_budget`     | BigInt\_for\_uint64 | Yes      | the gas budget; the transaction fails if the gas cost exceeds the budget                                |

## Response

Returns `TransactionBlockBytes`.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
