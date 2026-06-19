---
description: Retrieve the 20 largest token accounts for a specific SPL Token.
---

# getTokenLargestAccounts

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": "123",
    "method": "getTokenLargestAccounts",
     "params": [
       "jovWMTogqDpoEWUSFSns2Y9rkJkfJHsTgPWJgzCjwCF",
       {
         "commitment": "confirmed"
       }
     ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Name   | Description                                     |
| ------ | ----------------------------------------------- |
| pubkey | The unique identifier of the token to retrieve. |

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
