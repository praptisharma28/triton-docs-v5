---
description: Returns the Yellowstone gRPC service version, as a unary gRPC call.
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

# getVersion

## Request

{% tabs %}
{% tab title="grpcurl" %}
```bash
grpcurl \
  -d '{}' \
  -H "x-token: <your-token>" \
  <your-endpoint>.mainnet.rpcpool.com:443 \
  geyser.Geyser/GetVersion
```
{% endtab %}
{% endtabs %}

## Parameters

This call takes no parameters.

## Response

```json
{ "version": "{\"version\":\"6.1.0\"}" }
```

## Result

**`version`** · string: Version string for the gRPC service.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
