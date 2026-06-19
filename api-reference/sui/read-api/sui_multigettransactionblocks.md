---
description: >-
  Returns an ordered list of transaction responses The method will throw an
  error if the input contains any duplicate or the input size exceeds
  QUERY_MAX_RESULT_LIMIT
---

# sui\_multiGetTransactionBlocks

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_multiGetTransactionBlocks",
  "params": [
    [
      "EMqJqkQip6UaTkdff493ewAQNHGQFJwXDDn6m9CTgZzo",
      "Hn3B25vTQNTFAdThFkWvfkiAAUZxzwTaj4uEengu1ACX",
      "9vLSG9a4QcLcMdG1xCu6FRdXAjWWqvJHoHBCJfPMKkR9"
    ],
    {
      "showInput": true,
      "showRawInput": false,
      "showEffects": true,
      "showEvents": true,
      "showObjectChanges": false,
      "showBalanceChanges": false,
      "showRawEffects": false
    }
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type                            | Required | Description                                     |
| --------- | ------------------------------- | -------- | ----------------------------------------------- |
| `digests` | array                           | Yes      | A list of transaction digests.                  |
| `options` | TransactionBlockResponseOptions | No       | config options to control which fields to fetch |

## Response

Returns `Vec<SuiTransactionBlockResponse>`. (`array`)

The full `Vec<SuiTransactionBlockResponse>` response is large; call the method to see every field.
