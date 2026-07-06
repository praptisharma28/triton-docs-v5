---
description: Create an unsigned transaction to merge multiple coins into one coin.
---

# unsafe\_mergeCoins

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "unsafe_mergeCoins",
  "params": [
    "<signer>",
    "<primary_coin>",
    "<coin_to_merge>",
    "<gas>",
    "<gas_budget>"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter       | Type                | Required | Description                                                                                                |
| --------------- | ------------------- | -------- | ---------------------------------------------------------------------------------------------------------- |
| `signer`        | SuiAddress          | Yes      | the transaction signer's Sui address                                                                       |
| `primary_coin`  | ObjectID            | Yes      | the coin object to merge into, this coin will remain after the transaction                                 |
| `coin_to_merge` | ObjectID            | Yes      | the coin object to be merged, this coin will be destroyed, the balance will be added to `primary_coin`     |
| `gas`           | ObjectID            | No       | gas object to be used in this transaction, node will pick one from the signer's possession if not provided |
| `gas_budget`    | BigInt\_for\_uint64 | Yes      | the gas budget; the transaction fails if the gas cost exceeds the budget                                |

## Response

Returns `TransactionBlockBytes`.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
