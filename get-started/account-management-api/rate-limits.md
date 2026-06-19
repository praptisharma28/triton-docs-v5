---
description: Check the per-second, per-IP request limits applied to your endpoints.
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
    visible: false
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# Rate limits

## **Overview**

A rate limit caps how many requests per second an endpoint accepts. Triton enforces it per IP across all nodes to keep endpoints stable and protect against abusive traffic

Our goal is to allow legitimate application traffic to flow smoothly while mitigating the impact of aggressive bots.

Endpoints authenticated with a token get a more favourable rate limit than endpoints restricted by allowed origins, since tokens are typically used by backend servers (many users per IP) and allowed origins by browsers (one user per IP).

Rate limits do not affect pricing. Our products are billed by usage (per million requests plus per GB of bandwidth).

## **Get rate limits**

<mark style="color:red;">This API is only limited to account management API tokens created with the reseller role.</mark>

<mark style="color:blue;">`GET /api/v1/rate_tiers`</mark>

Returns all Rate Tier types that an Endpoint and Token can get created with.

```json
{
  "rate_tiers": [
    { "name": "free" },
    { "name": "tier1" },
    { "name": "tier2" },
    { "name": "tier3" },
    { "name": "dedi" }
  ]
} 
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
