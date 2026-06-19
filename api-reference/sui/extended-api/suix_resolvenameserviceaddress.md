---
description: Return the resolved address given resolver and name
---

# suix\_resolveNameServiceAddress

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_resolveNameServiceAddress",
  "params": [
    "example.sui"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type   | Required | Description         |
| --------- | ------ | -------- | ------------------- |
| `name`    | string | Yes      | The name to resolve |

## Response

Returns `SuiAddress`.

```json
"0x6710024f81dd33ab6833482ee8034e779a48e6ef635c7f856df4905022458bfb"
```
