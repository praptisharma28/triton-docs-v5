---
description: Get token prices from the Metis Swap API, Triton's hosted quoting engine.
---

# Metis /price

Returns the current price for a token, derived from Metis routing. Use it to display prices without building a full swap. Served at `/metis/price` on your Triton endpoint.

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl 'https://<your-endpoint>.mainnet.rpcpool.com/<your-token>/metis/price?ids=So11111111111111111111111111111111111111112'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Description                              |
| --------- | ---------------------------------------- |
| `ids`     | Comma-separated mint addresses to price. |

Metis mirrors the Jupiter price surface. For the full parameter set and response shape, see the [Jupiter price reference](https://dev.jup.ag/docs/price-api).

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
