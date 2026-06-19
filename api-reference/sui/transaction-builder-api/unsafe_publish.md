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
| `gas_budget`       | BigInt\_for\_uint64 | Yes      | the gas budget, the transaction will fail if the gas cost exceed the budget                                |

## Response

Returns `TransactionBlockBytes`.
