---
description: >-
  Return the resolved names given address, if multiple names are resolved, the
  first one is the primary name.
---

# suix\_resolveNameServiceNames

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_resolveNameServiceNames",
  "params": [
    "0x5cd6fa76ed1d18f05f15e35075252ddec4fb83621d55952d9172fcfcb72feae2",
    "0xd22bbb46f892c42d9ec0ae4de93e02c75973a51c17180798237326a58694a2cf",
    3
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type       | Required | Description            |
| --------- | ---------- | -------- | ---------------------- |
| `address` | SuiAddress | Yes      | The address to resolve |
| `cursor`  | ObjectID   | No       |                        |
| `limit`   | integer    | No       |                        |

## Response

Returns `Page<String,ObjectID>`. (`Page_for_String_and_ObjectID`)

```json
{
  "data": [
    "example.sui"
  ],
  "nextCursor": "0xd22bbb46f892c42d9ec0ae4de93e02c75973a51c17180798237326a58694a2cf",
  "hasNextPage": false
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
