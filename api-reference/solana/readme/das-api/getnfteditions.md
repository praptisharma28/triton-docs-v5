---
description: Get all printable editions for a master edition NFT mint.
---

# getNftEditions

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "id": "123",
    "jsonrpc": "2.0",
    "method": "getNftEditions",
    "params": {
      "mintAddress": "BUaiggSfm81ZRAeW572dNf1BhXCzZxaWrGNRm5PjSQzY",
      "page": 1,
      "limit": 100
    }
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Name          | Description                             | Required |
| ------------- | --------------------------------------- | -------- |
| `mintAddress` | The mint address of the master edition. | Yes      |
| `page`        | The current pagination page.            | No       |
| `limit`       | Number of results per page.             | No       |
| `cursor`      | Optional pagination cursor.             | No       |
| `before`      | Return results before the cursor.       | No       |
| `after`       | Return results after the cursor.        | No       |

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
