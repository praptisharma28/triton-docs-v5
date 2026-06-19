---
description: Subscribe to a stream of Sui transaction effects
---

# suix\_subscribeTransaction

> A WebSocket subscription. Connect with `wss://` and send this as a WebSocket message; updates stream back until you unsubscribe.

## Request

{% tabs %}
{% tab title="WebSocket message" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_subscribeTransaction",
  "params": [
    "<filter>"
  ]
}
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type              | Required | Description |
| --------- | ----------------- | -------- | ----------- |
| `filter`  | TransactionFilter | Yes      |             |

## Response

Returns `SuiTransactionBlockEffects`. (`TransactionBlockEffects`)

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
