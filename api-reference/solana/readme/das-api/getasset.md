---
description: >-
  This method retrieves detailed information about a specific digital asset/NFT.
  The response includes the asset's URL, collection, creators, authorities, and compression details.
---

# getAsset

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": "123",
    "method": "getAsset",
    "params": {
      "id": "7k7YqWc85BtyTx5UyvNnogv2jPHVe4BZ9HQsFLM8VnoJ"
    }
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Name | Description                                     | Required |
| ---- | ----------------------------------------------- | -------- |
| id   | The unique identifier of the asset to retrieve. | Yes      |

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
