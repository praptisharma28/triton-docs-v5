# Dedicated nodes

A dedicated node is single-tenant Triton infrastructure: your own endpoint with isolated capacity, open rate limits, and a fixed monthly price.

## What a dedicated node is

A dedicated node is a bare-metal server, or a cluster of them, that only you use. You get your own endpoint, a fixed monthly cost with no per-query metering, and capacity that no other customer shares, so there are no noisy neighbours.

Triton's shared infrastructure is the opposite trade: multi-tenant, globally distributed, and billed per usage. Shared suits front-ends, dashboards, and most production reads. Dedicated suits streaming, indexing, and latency-sensitive backends that want isolation, maximum throughput, and the lowest, steadiest latency.

## What dedicated nodes are for

Dedicated nodes are provisioned for **gRPC streaming** and other **single-purpose clusters** (for example a dedicated DAS, Photon, or custom-indexing cluster). Each cluster runs one job; mixed-purpose nodes are not offered.

This is the hybrid model most teams run: dedicated nodes carry the heavy backend workloads, streaming and indexing, while JSON-RPC reads (`getAccountInfo`, `getProgramAccounts`, `getTransaction`, and the rest) run on Triton's shared, globally distributed infrastructure. If you already run a dedicated node for JSON-RPC, it keeps working.

## When to choose one

Choose a dedicated node for the workloads it is built for: sustained gRPC streaming, heavy indexing, and ultra-low-latency paths. Reserving the full machine for your traffic gives you minimal latency, maximum throughput, and capacity no other tenant can touch.

The nuance worth knowing: consistency beats peak speed. A dedicated node's value is not a one-off low ping, it is predictable latency and low jitter under sustained load, because no neighbour's traffic burst can knock you off. Shared infrastructure is cost-effective and globally distributed, but it is rate-limited to protect other tenants, so some latency variance is unavoidable. For the full breakdown, see [shared vs dedicated RPC infrastructure](https://blog.triton.one/practical-guide-to-enterprise-solana-rpc-infrastructure/).

It is also a cost decision. On shared you pay for what you use; a dedicated node is a flat monthly price. If your streaming volume is large and steady, the fixed price is both cheaper and more predictable. Compare your own usage against the [pricing page](https://triton.one/pricing).

## What you can configure

* The job the cluster runs (gRPC streaming, DAS, Photon, indexing).
* The hardware tier.
* The region. Dedicated nodes are available in Europe, North America, Asia, and the Pacific.

## Getting one

Dedicated nodes are set up with our team, not self-serve. Start at [contact sales](https://triton.one/contact); the full flow is in the [quickstart](dedicated-nodes/quickstart.md). Pricing is a fixed monthly price per node with no metered or overage billing; see the [pricing page](https://triton.one/pricing).

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-rocket">:rocket:</i> <strong>Quickstart</strong></td><td>How to scope, provision, and connect a dedicated node.</td><td></td></tr><tr><td><i class="fa-list-check">:list-check:</i> <strong>Best practices</strong></td><td>Get the most out of a dedicated streaming node.</td><td></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
