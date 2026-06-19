---
description: Return a structured representation of Move module.
---

# sui\_getNormalizedMoveModule

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_getNormalizedMoveModule",
  "params": [
    "0x0047d5fa0a823e7d0ff4d55c32b09995a0ae1eedfee9c7b1354e805ed10ee3d0",
    "module"
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

## Response

Returns `SuiMoveNormalizedModule`.

```json
{
  "fileFormatVersion": 6,
  "address": "0x1639f3606a53f61f3a566963b3eac49fe3bb57d304a454ed2f4859b44f4e4918",
  "name": "module",
  "friends": [],
  "structs": {},
  "exposedFunctions": {}
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
