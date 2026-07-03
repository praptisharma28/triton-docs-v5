---
description: Returns the current slot, as a unary gRPC call.
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

# getSlot

## Request

{% tabs %}
{% tab title="grpcurl" %}
```bash
grpcurl \
  -d '{ "commitment": "FINALIZED" }' \
  -H "x-token: <your-token>" \
  <your-endpoint>.mainnet.rpcpool.com:443 \
  geyser.Geyser/GetSlot
```
{% endtab %}
{% endtabs %}

## Parameters

**`commitment`** · CommitmentLevel · optional

Commitment level for the query: `PROCESSED`, `CONFIRMED`, or `FINALIZED`.

## Response

```json
{ "slot": "317704512" }
```

## Result

**`slot`** · uint64: The current slot.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
