---
description: Generates an access list for a transaction.
---

# eth\_createAccessList

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_createAccessList",
  "params": [
    {
      "from": "0xaea8f8f781326bfe6a7683c2bd48dd6aa4d3ba63",
      "data": "0x608060806080608155"
    },
    "latest"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter     | Type               | Required | Description |
| ------------- | ------------------ | -------- | ----------- |
| `Transaction` | GenericTransaction | Yes      |             |
| `Block`       | BlockNumberOrTag   | No       |             |

## Response

Returns `Gas used`.

```json
{
  "accessList": [
    {
      "address": "0xa02457e5dfd32bda5fc7e1f1b008aa5979568150",
      "storageKeys": [
        "0x0000000000000000000000000000000000000000000000000000000000000081"
      ]
    }
  ],
  "gasUsed": "0x125f8"
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
