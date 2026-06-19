---
description: >-
  Private Sui nodes with isolated capacity, the full Sui service set, and your
  own endpoint.
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

# Dedicated nodes

A dedicated Sui node is a private node provisioned for a single customer, with isolated capacity and no shared rate or connection limits. It serves the same Sui services as the shared pool: gRPC (unary and streaming) and JSON-RPC while it remains available.

## What you get

* **Isolated capacity** on hardware reserved for you, with flexible rate and connection limits.
* **Your own endpoint**, a per-customer subdomain in the form `<your-slug>.sui.rpcpool.com`. Archival is served from a separate endpoint, `archive-<your-slug>.sui.rpcpool.com`.
* **The full Sui stack:** gRPC unary calls, gRPC streaming, and JSON-RPC (deprecating; see [Overview](./)).

## Hardware

Triton provisions dedicated Sui nodes well above Sui's minimum full-node requirements (8 cores / 16 vCPUs, 128 GB RAM, 4 TB NVMe), on high-memory bare-metal hardware.

## History and archival

A dedicated full node retains roughly the most recent two days (two epochs) of history, the same as the shared pool. For older data, query the [Archival storage and services](archival.md) endpoint, which serves the full history from genesis.

## Getting a dedicated node

Dedicated Sui nodes are provisioned on request. [Contact sales](https://triton.one/contact), or ask in your [customer dashboard](https://customers.triton.one) chat, to scope a node for your workload.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
