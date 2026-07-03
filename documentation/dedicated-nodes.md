---
description: >-
  Single-tenant Solana infrastructure with isolated capacity, a fixed monthly
  price, and your choice of region.
---

# Dedicated nodes

A dedicated node is single-tenant Triton infrastructure: a bare-metal server, or a cluster of them, that only you use. You get your own endpoint, isolated capacity no other customer shares (so there are no noisy neighbours), open rate limits, and a fixed monthly price with no per-query metering.

Dedicated nodes are provisioned for **gRPC streaming** and other **single-purpose clusters** (for example a dedicated DAS or custom-indexing cluster). Each cluster runs one job; mixed-purpose nodes are not offered.

## Shared vs dedicated

| | Shared infrastructure | Dedicated node |
| --- | --- | --- |
| Tenancy | Multi-tenant | Single-tenant, bare metal |
| Geo-distribution | Globally distributed, geo-routed | One region of your choice (Europe, North America, Asia, Pacific) |
| Auto-failover | Yes, across the global cluster | Yes, fails over to the shared cluster |
| Rate and connection limits | Flexible limits, tuned per plan | Open, no rate limits |
| Billing | Usage-based: `$0.08 / GB` plus per-call rates. Minimum deposit `$125`, usable across all services, valid 12 months | Fixed monthly price per node; streaming nodes start at `$2,900 / month`, no metering or overage |
| JSON-RPC reads | Served in place | Routed to the shared cluster and billed separately, per usage |

The cost cut-over is simple to estimate: at `$0.08 / GB`, streaming about 36 TB a month on shared costs the same as a `$2,900` dedicated node. Above that, or when you need guaranteed isolation, dedicated wins. Compare your own volume against the [pricing page](https://triton.one/pricing).

This is also why the hybrid model is what most teams run: dedicated nodes carry the heavy backend workloads, streaming and indexing, while JSON-RPC reads (`getAccountInfo`, `getProgramAccounts`, `getTransaction`, and the rest) run on Triton's shared, globally distributed infrastructure.

## Use cases

Choose a dedicated node for the workloads it is built for:

* **Sustained gRPC streaming** at high volume, where a fixed price beats metered bandwidth.
* **Heavy indexing** and other single-purpose clusters (DAS, custom indexes) that need full, uncontended throughput.
* **Ultra-low-latency backends.** The value is not a one-off low ping, it is predictable latency and low jitter under sustained load, because no neighbour's traffic burst can knock you off.

Not for front-ends, dashboards, or general production reads: shared infrastructure is cost-effective, globally distributed, and the right answer there. For the full breakdown, see [shared vs dedicated RPC infrastructure](https://blog.triton.one/practical-guide-to-enterprise-solana-rpc-infrastructure/).

## Features and benefits

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-server">:server:</i> <strong>Isolated capacity</strong></td><td>A bare-metal server only you use. No noisy neighbours, no contention, maximum throughput.</td><td></td></tr><tr><td><i class="fa-gauge-high">:gauge-high:</i> <strong>Open rate limits</strong></td><td>No rate or connection limits on your endpoint. Push it as hard as your workload needs.</td><td></td></tr><tr><td><i class="fa-coins">:coins:</i> <strong>Fixed monthly price</strong></td><td>Streaming nodes start at $2,900 / month with no metering or overage billing.</td><td></td></tr><tr><td><i class="fa-globe">:globe:</i> <strong>Your choice of region</strong></td><td>Europe, North America, Asia, or the Pacific, so you can colocate your backend next to the node.</td><td></td></tr><tr><td><i class="fa-shield">:shield:</i> <strong>Logging-policy control</strong></td><td>Pick the logging policy your legal or privacy requirements demand: Max Privacy, Enhanced Privacy (default), or GDPR Privacy.</td><td></td></tr><tr><td><i class="fa-arrows-rotate">:arrows-rotate:</i> <strong>Automatic failover</strong></td><td>If your node restarts or falls behind the tip, traffic fails over to the shared cluster and your stream keeps flowing.</td><td></td></tr></tbody></table>

## What you can configure

* The job the cluster runs (gRPC streaming, DAS, custom indexing) and its Geyser plugins.
* The hardware tier.
* The region: dedicated nodes are available in Europe, North America, Asia, and the Pacific.
* Access controls: token auth and allowed origins on your endpoint.
* The logging policy: **Max Privacy** (no request parameters or payloads logged), **Enhanced Privacy** (the default: method parameters may be logged, but a transaction's signer is never linked to an originating IP), or **GDPR Privacy** (full logging for support, legal, or compliance needs). See [privacy and data protection](https://app.gitbook.com/s/ACym6ZbIwDBDKhyKgDGy/privacy-and-security).

## Getting started

Dedicated nodes are set up with our team, not self-serve.

{% stepper %}
{% step %}
### Talk to us

Reach out through [contact sales](https://triton.one/contact). A short know-your-customer step collects your contact and location details.
{% endstep %}

{% step %}
### Scope your node

A discovery call covers your streaming volume, regions, and the job the cluster will run (gRPC streaming, DAS, or indexing). You agree the specs, region, and number of nodes before anything is deployed.
{% endstep %}

{% step %}
### Provisioning

Hardware is ordered to spec and set up as a dedicated subscription in your [customer dashboard](https://customers.triton.one). Because nodes are built to order rather than kept in stock, expect a short lead time. There are no same-day trials.
{% endstep %}

{% step %}
### Connect

Your node has its own endpoint, `https://<your-endpoint>.mainnet.rpcpool.com`. Point your gRPC client at it with your endpoint URL and token, exactly as you would any Triton endpoint. For client code in TypeScript, Rust, and Go, see [Dragon's Mouth gRPC](https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/dragon-s-mouth-grpc).
{% endstep %}
{% endstepper %}

Pricing is a fixed monthly price per node, starting at `$2,900 / month` for streaming nodes, with no metered or overage billing; see the [pricing page](https://triton.one/pricing).

## Best practices

* **Put all your streaming traffic on the dedicated endpoint.** Leaving streams on a shared endpoint means metered charges and defeats the point of a fixed-cost node.
* **Run your backend close to the node.** gRPC streams are backend-to-backend, so geographic distance is the main latency factor.
* **Filter to cut volume.** Server-side filters (skip votes, restrict to the accounts and programs you care about) reduce the bandwidth you pull and help you stay on the chain tip under load. See [streaming best practices](https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/best-practices).

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-radio">:radio:</i> <strong>Dragon's Mouth gRPC</strong></td><td>Connect your stream and see every subscription type and client.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/dragon-s-mouth-grpc">Dragon's Mouth gRPC</a></td></tr><tr><td><i class="fa-list-check">:list-check:</i> <strong>Streaming best practices</strong></td><td>Filtering, reconnect, and commitment guidance for streaming nodes.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/best-practices">Streaming best practices</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
