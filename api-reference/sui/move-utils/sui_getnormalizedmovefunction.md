---
description: Return a structured representation of Move function
---

# sui\_getNormalizedMoveFunction

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_getNormalizedMoveFunction",
  "params": [
    "0x9c4eb6769ca8b6a23efeb7298cf0a8d0b837b78749c2cfc711c42036cc6b7621",
    "moduleName",
    "functionName"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter       | Type     | Required | Description |
| --------------- | -------- | -------- | ----------- |
| `package`       | ObjectID | Yes      |             |
| `module_name`   | string   | Yes      |             |
| `function_name` | string   | Yes      |             |

## Response

Returns `SuiMoveNormalizedFunction`.

```json
{
  "visibility": "Public",
  "isEntry": false,
  "typeParameters": [
    {
      "abilities": [
        "Store",
        "Key"
      ]
    }
  ],
  "parameters": [
    "U64"
  ],
  "return": [
    "U64"
  ]
}
```
