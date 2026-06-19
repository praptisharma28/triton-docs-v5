---
description: Create an unsigned batched transaction.
---

# unsafe\_batchTransaction

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "unsafe_batchTransaction",
  "params": [
    "<signer>",
    "<single_transaction_params>",
    "<gas>",
    "<gas_budget>",
    "<txn_builder_mode>"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter                   | Type                           | Required | Description                                                                                                |
| --------------------------- | ------------------------------ | -------- | ---------------------------------------------------------------------------------------------------------- |
| `signer`                    | SuiAddress                     | Yes      | the transaction signer's Sui address                                                                       |
| `single_transaction_params` | array                          | Yes      | list of transaction request parameters                                                                     |
| `gas`                       | ObjectID                       | No       | gas object to be used in this transaction, node will pick one from the signer's possession if not provided |
| `gas_budget`                | BigInt\_for\_uint64            | Yes      | the gas budget, the transaction will fail if the gas cost exceed the budget                                |
| `txn_builder_mode`          | SuiTransactionBlockBuilderMode | No       | Whether this is a regular transaction or a Dev Inspect Transaction                                         |

## Response

Returns `TransactionBlockBytes`.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
