---
description: Triton billing methods, infrastructure options, and per-service rates.
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

# Plans and billing

We've compared the billing and infrastructure types we offer, so you can pick the combination that best fits your workload.

## Plans: shared vs dedicated

|                                | Shared infrastructure                                                                      | Dedicated node                                                                                           |
| ------------------------------ | ------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------- |
| **Geo-distribution**           | Routed across 10 cities on three continents                                                | Deployed in one location of your choosing                                                                |
| **Feature coverage**           | Every service and network is included                                                      | Single-network streaming nodes and custom setups: [contact sales](https://triton.one/contact) to discuss |
| **Latency**                    | Low: routed to the nearest data centre                                                     | Minimal: if colocated with your backend                                                                  |
| **Spike absorption**           | Shared clusters are sized to absorb network-wide spikes seamlessly                         | Fixed node capacity, dedicated to your traffic; auto-failover to shared cluster                          |
| **Noisy neighbours**           | Low: we overprovision our infrastructure to avoid network-wide load affecting your traffic | None, the node serves only your traffic                                                                  |
| **Minimum cost**               | $125 (Pay as you go, valid for 12 months)                                                  | Fixed monthly price, varies by workload                                                                  |
| **Connection and rate limits** | Flexible, can be raised at no extra cost                                                   | None                                                                                                     |
| **Sign up**                    | Self-onboard at [customers.triton.one](https://customers.triton.one/users/sign-up)         | [Contact sales](https://triton.one/contact)                                                              |

Shared infrastructure is the best fit for most workloads. Choose a dedicated node to colocate your backend for minimal latency (for traders, market makers, and bots), or when you have a custom high-throughput workload that requires guaranteed, isolated capacity.

## Billing: pay-as-you-go vs invoiced

|                      | PAYG                                                                               | Invoiced                                                                  |
| -------------------- | ---------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| **Payment**          | Stablecoins only                                                                   | Card, USDC, wire, hel.io                                                  |
| **Billing**          | Drawn from your prepaid balance                                                    | Billed on the 1st of each month for the upcoming month                    |
| **Overages**         | Not allowed. Set up low balance alerts to avoid service interruptions              | Billed in $100 increments at the same base rates on the 1st of next month |
| **Feature coverage** | Every service and network is included                                              | Every service and network is included                                     |
| **Cancellation**     | None, your deposit is valid for 12 months                                          | 1 calendar month notice                                                   |
| **Minimum cost**     | $125 prepaid deposit                                                               | $125 monthly invoice                                                      |
| **Sign up**          | Self-onboard at [customers.triton.one](https://customers.triton.one/users/sign-up) | [Contact sales](https://triton.one/contact)                               |

Pay as you go is the fastest to get started on, but most teams running steady production traffic switch to monthly invoicing once usage stabilises to get:

* **Overages at the base rates.** If you go over the estimate, you get a follow-up invoice at the same rates, with no sudden service interruption.
* **Predictable bills.** Same line items each month, sized around the previous month's volume and adjustable as usage shifts.

To switch to monthly invoicing, click the chat icon at the bottom right of your [customer dashboard](https://customers.triton.one) or [contact sales](https://triton.one/contact).

## How pay-as-you-go works

During onboarding, you fund a prepaid, non-refundable balance in stablecoins. It covers every service and network, and your usage is billed against it as you go.

Your deposit stays valid for 12 months, so you can try every feature at your own pace (rates below). [Start from $125](http://customers.triton.one/onboarding) and top up anytime from your [customer dashboard](https://customers.triton.one).

## Per-service rates

Every subscription includes the full feature set across all networks we support. Nothing is tier-gated by how much you deposit or involves hidden minimums, limits or fees.

On shared infrastructure, you pay as you go at the following rates:

{% tabs %}
{% tab title="Solana" %}
<table><thead><tr><th width="322.87109375">Service</th><th>Rate</th></tr></thead><tbody><tr><td>Dragon's Mouth gRPC, Fumarole, Whirligig, WebSockets, other streaming</td><td>$0.08 / GB bandwidth</td></tr><tr><td>Standard RPC, unary gRPC, indexed accounts, ledger queries (plus gTFA)</td><td>$10 / million calls + $0.08 / GB</td></tr><tr><td>Streaming services, Titan Prime API</td><td>$0.08 / GB bandwidth</td></tr><tr><td>Metaplex DAS API</td><td>$50 / million calls + $0.08 / GB</td></tr><tr><td>Metis API</td><td>$80 / million calls + $0.08 / GB</td></tr></tbody></table>
{% endtab %}

{% tab title="Sui" %}
| Service                       | Rate                             |
| ----------------------------- | -------------------------------- |
| Unary gRPC, Walrus, Seal APIs | $10 / million calls + $0.08 / GB |
| Archival gRPC service         | $10 / million calls + $0.12 / GB |
| gRPC streaming                | $0.08 / GB bandwidth             |
{% endtab %}

{% tab title="Monad" %}
| Service             | Rate                             |
| ------------------- | -------------------------------- |
| Standard RPC        | $10 / million calls + $0.08 / GB |
| Websocket streaming | $0.08 / GB bandwidth             |
{% endtab %}
{% endtabs %}

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-gauge">:gauge:</i> <strong>Rate and connection limits</strong></td><td>Per-endpoint and method rate limits, plus streaming connection caps.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/rate-and-connection-limits">https://kate-6.gitbook.io/triton-one-docs-v5/rate-and-connection-limits</a></td></tr><tr><td><i class="fa-user-gear">:user-gear:</i> <strong>Customer dashboard overview</strong></td><td>Customer dashboard tour: endpoints, billing, team, and support.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/platform-overview">https://kate-6.gitbook.io/triton-one-docs-v5/platform-overview</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
