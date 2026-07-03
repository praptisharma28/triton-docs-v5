---
description: >-
  Single-purpose Solana node with isolated capacity, flat monthly price, and no
  rate and connection limits.
---

# Dedicated nodes

We offer premium bare-metal private nodes for the lowest latency, highest throughput, no noisy neighbours, and fixed costs. The most common service we run is streaming nodes, but we can also provision other single-purpose clusters (for example a dedicated DAS or custom-indexing cluster). Mixed-purpose nodes (e.g. JSON-RPC + gRPC streaming) are not offered.

## Use cases

Choose a dedicated node for:

* **Sustained gRPC streaming** at high volume, where a fixed price is a priority.
* **Heavy indexing workloads** (DAS / History) that need full, uncontended throughput.
* **Latency-sensitive backends** that require colocation with the data source for predictable speed and minimal jitter at all times.

Not for front-ends, dashboards, or general production reads: shared infrastructure is cost-effective, globally distributed, and the right answer there. For the full breakdown, see [shared vs dedicated RPC infrastructure](https://blog.triton.one/practical-guide-to-enterprise-solana-rpc-infrastructure/).

## Features and benefits

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-gauge-high">:gauge-high:</i> <strong>Open rate limits</strong></td><td>No rate or connection limits on your endpoint. Push it as hard as your workload needs.</td><td></td></tr><tr><td><i class="fa-globe">:globe:</i> <strong>Your choice of region</strong></td><td>Europe, North America, Asia, or the Pacific, so you can colocate your backend next to the node.</td><td></td></tr><tr><td><i class="fa-shield">:shield:</i> <strong>Logging-policy control</strong></td><td>Pick the logging policy your legal or privacy requirements demand: Max Privacy, Enhanced Privacy (default), or GDPR Privacy.</td><td></td></tr><tr><td><i class="fa-arrows-rotate">:arrows-rotate:</i> <strong>Automatic failover</strong></td><td>If your node restarts or falls behind the tip, traffic fails over to the shared cluster and your stream keeps flowing.</td><td></td></tr></tbody></table>

## Shared vs dedicated

| | Shared infrastructure | Dedicated node |
| --- | --- | --- |
| Tenancy | Multi-tenant | Single-tenant, bare metal |
| Geo-distribution | Globally distributed, geo-routed | One region of your choice (Europe, North America, Asia, Pacific) |
| Auto-failover | Yes, across the global cluster | Yes, fails over to the shared cluster |
| Rate and connection limits | Flexible limits, tuned per plan | Open, no rate limits |
| Billing | Usage-based: `$0.08 / GB` plus per-call rates. Minimum deposit `$125`, usable across all services, valid 12 months | Fixed monthly price per node; streaming nodes start at `$2,900 / month` with no overages |
| JSON-RPC reads | Served in place | Routed to the shared cluster and billed separately, per usage |

The cost cut-over is simple to estimate: at `$0.08 / GB`, streaming about 36 TB a month on shared costs the same as a `$2,900` dedicated node. Above that, or when you need guaranteed isolation, dedicated wins. Compare your own volume against the [pricing page](https://triton.one/pricing).

This is also why the hybrid model is what most teams run: dedicated nodes carry the heavy backend workloads, streaming and indexing, while JSON-RPC reads (`getAccountInfo`, `getProgramAccounts`, `getTransaction`, and the rest) run on Triton's shared, globally distributed infrastructure.

## Getting started

Dedicated nodes are set up with our team, not self-serve.

{% stepper %}
{% step %}
### Talk to us

Reach out through [contact sales](https://triton.one/contact) and we'll talk through the best options with you.
{% endstep %}

{% step %}
### Scope your node

A discovery call covers your streaming volume, regions, and the job the cluster will run (gRPC streaming, DAS, or indexing). We'll agree on the specs, region, and number of nodes being deployed.
{% endstep %}

{% step %}
### Provisioning

Hardware is ordered to spec and set up as a dedicated subscription in your [customer dashboard](https://customers.triton.one).
{% endstep %}

{% step %}
### Connect

Your node has its own endpoint, `https://<your-endpoint>.mainnet.rpcpool.com`. Point your gRPC client at it with your endpoint URL and token, exactly as you would any Triton endpoint. For client code in TypeScript, Rust, and Go, see [Dragon's Mouth gRPC](https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/dragon-s-mouth-grpc).
{% endstep %}
{% endstepper %}


## Best practices

* **Put all your streaming traffic on the dedicated endpoint.** Leaving streams on a shared endpoint means metered charges and defeats the point of a fixed-cost node.
* **Run your backend close to the node.** gRPC streams are backend-to-backend, so geographic distance is the main latency factor.
* **Filter to cut volume.** Server-side filters (skip votes, restrict to the accounts and programs you care about) reduce the bandwidth you pull and help you stay on the chain tip under load. See [streaming best practices](https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/best-practices).

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-tower-broadcast">:tower-broadcast:</i> <strong>Dragon's Mouth gRPC</strong></td><td>Sub-slot real-time updates for accounts, transactions, slots, and blocks via gRPC.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/dragon-s-mouth-grpc">Dragon's Mouth gRPC</a></td></tr><tr><td><i class="fa-list-check">:list-check:</i> <strong>Best practices</strong></td><td>Filtering, reconnect, and commitment guidance across the streaming services.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/best-practices">Best practices</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
