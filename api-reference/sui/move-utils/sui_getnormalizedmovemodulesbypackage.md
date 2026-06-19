---
description: Return structured representations of all modules in the given package
---

# sui\_getNormalizedMoveModulesByPackage

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_getNormalizedMoveModulesByPackage",
  "params": [
    "0x61630d3505f8905a0f4d42c6ff39a78a6ba2b28f68a3299ec3417bbabc6717dc"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type     | Required | Description |
| --------- | -------- | -------- | ----------- |
| `package` | ObjectID | Yes      |             |

## Response

Returns `BTreeMap<String,SuiMoveNormalizedModule>`. (`object`)

```json
{
  "fileFormatVersion": 6,
  "address": "0x800105867da4655eca6d9eb1258bfd1ad92af329a07781ee71e60065e00f2de9",
  "name": "module",
  "friends": [],
  "structs": {},
  "exposedFunctions": {}
}
```
