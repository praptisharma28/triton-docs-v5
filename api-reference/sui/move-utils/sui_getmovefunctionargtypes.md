---
description: Return the argument types of a Move function, based on normalized Type.
---

# sui\_getMoveFunctionArgTypes

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_getMoveFunctionArgTypes",
  "params": [
    "0xa0a7b108f5023b7356f2c6a4be6f058e267aae38e08260c7d519d8641897490c",
    "suifrens",
    "mint"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter  | Type     | Required | Description |
| ---------- | -------- | -------- | ----------- |
| `package`  | ObjectID | Yes      |             |
| `module`   | string   | Yes      |             |
| `function` | string   | Yes      |             |

## Response

Returns `Vec<MoveFunctionArgType>`. (`array`)

```json
[
  {
    "Object": "ByMutableReference"
  },
  "Pure",
  "Pure",
  {
    "Object": "ByValue"
  },
  {
    "Object": "ByImmutableReference"
  },
  {
    "Object": "ByValue"
  },
  {
    "Object": "ByMutableReference"
  }
]
```
