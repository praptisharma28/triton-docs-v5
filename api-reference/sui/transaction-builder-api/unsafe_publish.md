---
description: Create an unsigned transaction to publish a Move package.
---

# unsafe\_publish

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "unsafe_publish",
  "params": [
    "<sender>",
    "<compiled_modules>",
    "<dependencies>",
    "<gas>",
    "<gas_budget>"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter          | Type                | Required | Description                                                                                                |
| ------------------ | ------------------- | -------- | ---------------------------------------------------------------------------------------------------------- |
| `sender`           | SuiAddress          | Yes      | the transaction signer's Sui address                                                                       |
| `compiled_modules` | array               | Yes      | the compiled bytes of a Move package                                                                       |
| `dependencies`     | array               | Yes      | a list of transitive dependency addresses that this set of modules depends on.                             |
| `gas`              | ObjectID            | No       | gas object to be used in this transaction, node will pick one from the signer's possession if not provided |
| `gas_budget`       | BigInt\_for\_uint64 | Yes      | the gas budget; the transaction fails if the gas cost exceeds the budget                                |

## Response

Returns `TransactionBlockBytes`.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
