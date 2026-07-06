---
description: Private Monad nodes with isolated capacity, provisioned on request.
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

# Dedicated nodes

A dedicated Monad node is a private server provisioned for a single purpose, delivering isolated capacity and no shared rate or connection limits. It serves the same APIs as the shared Monad pool: JSON-RPC over HTTPS and the WebSocket (WSS) API.

## What you get

* **No rate or connection limits**, as all the CPU and bandwidth are reserved for you.
* **Your own endpoint**, a per-customer subdomain in the form `<your-endpoint>.<network>.monad.rpcpool.com`.
* **The full Monad API set:** JSON-RPC over HTTPS and the WebSocket (WSS) API, with WebSocket streaming unmetered.
* **Your choice of region**: Europe, North America, Asia, or the Pacific, colocated near your systems.
* **Access controls**: token auth and allowed origins on your endpoint.
* **Your logging policy**: Max Privacy (no request parameters or payloads logged), Enhanced Privacy (the default: method parameters may be logged, but a transaction's signer is never linked to an originating IP), or GDPR Privacy (full logging for support, legal, or compliance needs). See [privacy and data protection](https://app.gitbook.com/s/ACym6ZbIwDBDKhyKgDGy/privacy-and-security).

## Pricing

Dedicated Monad nodes start at `$2,900 / month` per node. WebSocket streaming is unmetered and included in the node price, with no overage fees.

JSON-RPC on a dedicated node is billed at the standard metered rates: `$0.08 / GB` bandwidth plus `$10 / million` calls.

## Getting started

Dedicated nodes are set up with our team, not self-serve.

{% stepper %}
{% step %}
### Talk to us

Reach out through [contact sales](https://triton.one/contact), or ask in your [customer dashboard](https://customers.triton.one) chat, and we'll talk through the best options with you.
{% endstep %}

{% step %}
### Scope your node

A discovery call covers your traffic volume, regions, and workload. We'll agree on the specs, region, and number of nodes being deployed.
{% endstep %}

{% step %}
### Provisioning

Hardware is ordered to spec and set up as a dedicated subscription in your [customer dashboard](https://customers.triton.one).
{% endstep %}

{% step %}
### Connect

Your node has its own endpoint, `<your-endpoint>.<network>.monad.rpcpool.com`. Point your client at it with your endpoint URL and token, exactly as you would any Triton endpoint. For requests and examples, see the [Overview](./).
{% endstep %}
{% endstepper %}

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
