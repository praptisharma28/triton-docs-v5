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

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
