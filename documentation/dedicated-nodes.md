---
description: >-
  Single-tenant Solana infrastructure with isolated capacity, a fixed monthly
  price, and your choice of region.
---

# Dedicated nodes

A dedicated node is single-tenant Triton infrastructure: a bare-metal server, or a cluster of them, that only you use. You get your own endpoint, isolated capacity no other customer shares (so there are no noisy neighbours), open rate limits, and a fixed monthly price with no per-query metering.

Triton's shared infrastructure is the opposite trade: multi-tenant, globally distributed, and billed per usage. Shared suits front-ends, dashboards, and most production reads. Dedicated suits streaming, indexing, and latency-sensitive backends that want isolation, maximum throughput, and the lowest, steadiest latency.

Dedicated nodes are provisioned for **gRPC streaming** and other **single-purpose clusters** (for example a dedicated DAS or custom-indexing cluster). Each cluster runs one job; mixed-purpose nodes are not offered.

This is the hybrid model most teams run: dedicated nodes carry the heavy backend workloads, streaming and indexing, while JSON-RPC reads (`getAccountInfo`, `getProgramAccounts`, `getTransaction`, and the rest) run on Triton's shared, globally distributed infrastructure.

## When to choose one

Choose a dedicated node for the workloads it is built for: sustained gRPC streaming, heavy indexing, and ultra-low-latency paths. The value is not a one-off low ping, it is predictable latency and low jitter under sustained load, because no neighbour's traffic burst can knock you off. Shared infrastructure is cost-effective and globally distributed, but it is rate-limited to protect other tenants, so some latency variance is unavoidable. For the full breakdown, see [shared vs dedicated RPC infrastructure](https://blog.triton.one/practical-guide-to-enterprise-solana-rpc-infrastructure/).

It is also a cost decision. On shared you pay for what you use; a dedicated node is a flat monthly price. If your streaming volume is large and steady, the fixed price is both cheaper and more predictable. Compare your usage against the [pricing page](https://triton.one/pricing).

## What you can configure

* The job the cluster runs (gRPC streaming, DAS, custom indexing) and its Geyser plugins.
* The hardware tier.
* The region: dedicated nodes are available in Europe, North America, Asia, and the Pacific.
* Access controls: token auth and allowed origins on your endpoint.

## Getting started

Dedicated nodes are set up with our team, not self-serve.

1. **Talk to us.** Reach out through [contact sales](https://triton.one/contact). A short know-your-customer step collects your contact and location details.
2. **Scope your node.** A discovery call covers your streaming volume, regions, and the job the cluster will run (gRPC streaming, DAS, or indexing). You agree the specs, region, and number of nodes before anything is deployed.
3. **Provisioning.** Hardware is ordered to spec and set up as a dedicated subscription in your [customer dashboard](https://customers.triton.one). Because nodes are built to order rather than kept in stock, expect a short lead time. There are no same-day trials.
4. **Connect.** Your node has its own endpoint, `https://<your-endpoint>.mainnet.rpcpool.com`. Point your gRPC client at it with your endpoint URL and token, exactly as you would any Triton endpoint. For client code in TypeScript, Rust, and Go, see [Dragon's Mouth gRPC](../real-time-streaming/dragon-s-mouth-grpc).

Pricing is a fixed monthly price per node with no metered or overage billing; see the [pricing page](https://triton.one/pricing).

## Best practices

* **Put all your streaming traffic on the dedicated endpoint.** Leaving streams on a shared endpoint means metered charges and defeats the point of a fixed-cost node.
* **Run your backend close to the node.** gRPC streams are backend-to-backend, so geographic distance is the main latency factor. Place your service in the same region as your node rather than relying on geo-distribution.
* **Use the regional endpoint** where a product has one, connecting to the regional hostname rather than the shared geo-routed one.
* **Filter to cut volume.** Server-side filters (skip votes, restrict to the accounts and programs you care about) reduce both bandwidth and cost. See [streaming best practices](../real-time-streaming/best-practices).
* **Failover is automatic.** Nodes fail over into a backup pool, so a single node restarting or briefly falling behind the chain tip does not interrupt your stream.

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-radio">:radio:</i> <strong>Dragon's Mouth gRPC</strong></td><td>Connect your stream and see every subscription type and client.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/dragon-s-mouth-grpc">Dragon's Mouth gRPC</a></td></tr><tr><td><i class="fa-list-check">:list-check:</i> <strong>Streaming best practices</strong></td><td>Filtering, reconnect, and commitment guidance for streaming nodes.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/best-practices">Streaming best practices</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
