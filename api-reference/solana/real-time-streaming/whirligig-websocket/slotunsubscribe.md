---
description: Cancel a slotSubscribe websocket subscription.
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# slotUnsubscribe

## Request

{% tabs %}
{% tab title="jsonc" %}
```
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "slotUnsubscribe",
  "params": [0]
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`subscription id`** · integer · required

Subscription id returned by `slotSubscribe`.

## Response

```json
{
  "jsonrpc": "2.0",
  "result": true,
  "id": 1
}
```

## Result

Returns `true` if the subscription was removed. Returns an error if the subscription id is unknown.

***

_Adapted from the_ [_Solana `slotUnsubscribe` reference_](https://solana.com/docs/rpc/websocket/slotunsubscribe)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
