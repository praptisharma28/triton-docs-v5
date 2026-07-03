---
description: >-
  Private Sui node with isolated capacity, a flat monthly price, and the full
  Sui service set on your own endpoint.
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

We offer premium bare-metal private Sui nodes for the lowest latency, highest throughput, no noisy neighbours, and fixed costs. A dedicated node serves the same gRPC Sui service as the shared infrastructure at a flat monthly price, while archival requests route to the archival storage endpoints on shared infrastructure.

## What you get

* **Flexible rate and connection limits** on hardware reserved for you.
* **Your own endpoint**, a per-customer subdomain in the form `<your-endpoint>.sui.rpcpool.com`. Archival is served from a separate endpoint, `archive-<your-endpoint>.sui.rpcpool.com`.
* **The full Sui stack:** gRPC unary calls, gRPC streaming, and JSON-RPC (deprecating; see [Overview](./)).
* **Your choice of region**: Europe, North America, Asia, or the Pacific, colocated near your systems.
* **Access controls**: token auth and allowed origins on your endpoint.
* **Your logging policy**: Max Privacy (no request parameters or payloads logged), Enhanced Privacy (the default: method parameters may be logged, but a transaction's signer is never linked to an originating IP), or GDPR Privacy (full logging for support, legal, or compliance needs). See [privacy and data protection](https://app.gitbook.com/s/ACym6ZbIwDBDKhyKgDGy/privacy-and-security).

## Hardware

Triton provisions dedicated Sui nodes well above Sui's minimum full-node requirements (8 cores / 16 vCPUs, 128 GB RAM, 4 TB NVMe), on high-memory bare-metal hardware.

## History and archival

A dedicated full node retains roughly the most recent two days (two epochs) of history, the same as the shared pool. For older data, query the [Archival storage and services](https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/sui/archival) endpoint, which serves the full history from genesis.

## Pricing

Dedicated Sui nodes start at `$2,000 / month` per node. gRPC streaming is unmetered and included in the node price, with no overage fees. Included with every dedicated endpoint: isolated performance dedicated to your traffic, custom geolocated deployment, advanced controls and tuning for your workload, GeoDNS routing and automatic failover, 1-1 support from senior engineers, and the Seal and Walrus APIs.

Non-streaming services on a dedicated node are billed at the standard metered rates:

| Service                                   | Rate                                        |
| ----------------------------------------- | ------------------------------------------- |
| Standard RPC (JSON-RPC, gRPC unary calls) | `$0.08 / GB` bandwidth + `$10 / million` calls |
| Seal permissioned server, Walrus storage  | `$0.08 / GB` bandwidth + `$10 / million` calls |
| Archival gRPC service                     | `$0.12 / GB` bandwidth + `$10 / million` calls |

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

Your node has its own endpoint, `<your-endpoint>.sui.rpcpool.com`. Point your gRPC client at it with your endpoint URL and token, exactly as you would any Triton endpoint. For requests and client setup, see [gRPC](grpc.md).
{% endstep %}
{% endstepper %}

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
