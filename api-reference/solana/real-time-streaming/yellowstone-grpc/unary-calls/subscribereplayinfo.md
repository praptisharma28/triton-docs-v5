---
description: >-
  Returns the first slot available for replay on a subscription, as a unary gRPC
  call.
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

# subscribeReplayInfo

## Request

{% tabs %}
{% tab title="grpcurl" %}
```bash
grpcurl \
  -d '{}' \
  -H "x-token: <your-token>" \
  <your-endpoint>.mainnet.rpcpool.com:443 \
  geyser.Geyser/SubscribeReplayInfo
```
{% endtab %}
{% endtabs %}

## Parameters

This call takes no parameters.

## Response

```json
{ "firstAvailable": "317690000" }
```

## Result

**`firstAvailable`** · uint64: First slot available for replay. May be absent if no replay buffer is configured.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
