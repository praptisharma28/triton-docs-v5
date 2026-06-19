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
