---
description: Return a structured representation of Move struct
---

# sui\_getNormalizedMoveStruct

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_getNormalizedMoveStruct",
  "params": [
    "0xc95b9e341bc3aba1654bdbad707dcd773bd6309363447ef3fe58a960de92aa93",
    "module",
    "StructName"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter     | Type     | Required | Description |
| ------------- | -------- | -------- | ----------- |
| `package`     | ObjectID | Yes      |             |
| `module_name` | string   | Yes      |             |
| `struct_name` | string   | Yes      |             |

## Response

Returns `SuiMoveNormalizedStruct`.

```json
{
  "abilities": {
    "abilities": [
      "Store",
      "Key"
    ]
  },
  "typeParameters": [],
  "fields": []
}
```
