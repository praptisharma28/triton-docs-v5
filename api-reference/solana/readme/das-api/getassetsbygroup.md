---
description: The assets in a group, such as a collection. Temporarily suspended.
---

# getAssetsByGroup

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": "123",
    "method": "getAssetsByGroup",
    "params": {
        "groupKey": "collection",
        "groupValue": "BUjZjAS2vbbb65g7Z1Ca9ZRVYoJscURG5L3AkVvHP9ac",
        "page": 1,
        "limit": 50
    }
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Name       | Description                                                                                                                                                                                                               | Required |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| groupKey   | The key of the group (e.g., `"collection"`).                                                                                                                                                                              | Yes      |
| groupValue | The value of the group.                                                                                                                                                                                                   | Yes      |
| sortBy     | Sorting criteria. This is specified as an object `{ sortBy: <value>, sortDirection: <value> }`, where `sortBy` is one of `["created", "updated", "recentAction", "none"]` and `sortDirection` is one of `["asc", "desc"]` | No       |
| page       | The index of the "page" to retrieve.                                                                                                                                                                                      | No       |
| before     | Fetch assets before the given ID.                                                                                                                                                                                         | No       |
| after      | Fetch assets after the given ID.                                                                                                                                                                                          | No       |

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
