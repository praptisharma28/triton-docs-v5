---
description: Return total supply for a coin.
---

# suix\_getTotalSupply

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_getTotalSupply",
  "params": [
    "0xe5c651321915b06c81838c2e370109b554a448a78d3a56220f798398dde66eab::acoin::ACOIN"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter   | Type   | Required | Description                                                                           |
| ----------- | ------ | -------- | ------------------------------------------------------------------------------------- |
| `coin_type` | string | Yes      | type name for the coin (e.g., 0x168da5bf1f48dafc111b0a488fa454aca95e0b5e::usdc::USDC) |

## Response

Returns `Supply`.

```json
{
  "value": "12023692"
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
