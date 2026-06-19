---
description: Triton's points of presence, automatic routing, and region pinning
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

# Endpoints, regions, and routing

We use BGP Anycast to serve all of Triton's regions from a single endpoint. Your requests automatically reach the nearest node, backed by built-in health checks and failover, with no routing logic needed on your end.

* Your endpoint connects to a high-availability cluster of load balancers
* The load balancers spread your requests across healthy nodes
* If a node or data centre goes down, your traffic is automatically rerouted to a healthy capacity

## Regions and points of presence

Our shared infrastructure clusters run on 3 continents across 10 cities:

| Region                   | Cities                                       |
| ------------------------ | -------------------------------------------- |
| United States of America | Dallas, Salt Lake City, New York, Pittsburgh |
| Europe                   | Amsterdam, Dublin, Frankfurt, London         |
| Asia Pacific             | Singapore, Tokyo                             |

## Endpoint URL format

You can find your endpoints and secret tokens in the customer dashboard. Each chain has its own hostname:

* Solana: `https://<your-endpoint>.<network>.rpcpool.com/<your-token>`
* Sui: `https://<your-endpoint>.<network>.sui.rpcpool.com/<your-token>`
* Monad: `https://<your-endpoint>.<network>.monad.rpcpool.com/<your-token>`

| Chain  | Networks                 |
| ------ | ------------------------ |
| Solana | Mainnet, Testnet, Devnet |
| Sui    | Mainnet, Testnet         |
| Monad  | Mainnet, Testnet         |

The same endpoint serves every protocol on that chain:

* **JSON-RPC** over HTTPS, with token or allowed origin authentication.
* **WebSocket** (Solana and Monad) over `wss://` , with token or allowed-origin authentication.
* **gRPC** over HTTPS, with your token as an `x-token` header for authentication.

## When to pin a region

Anycast to the nearest PoP is the right default for most workloads. To pin your traffic to a specific region or data centre, contact support, and we'll point your endpoint there.

**Fumarole is addressed by region directly.** Connect to `ams.rpcpool.com` (Europe) or `nyc.rpcpool.com` (US). Its subscriptions are region-stateful and do not carry across regions.

**Dedicated nodes** run at a data centre of your choice, with your backend colocated for minimal latency.

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-lock">:lock:</i> <strong>Auth and security</strong></td><td>Authenticate requests and lock your endpoint to trusted origins.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/authentication">https://kate-6.gitbook.io/triton-one-docs-v5/authentication</a></td></tr><tr><td><i class="fa-rocket">:rocket:</i> <strong>Solana overview</strong></td><td>Explore advanced Solana tooling and start integrating it into your build.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/readme">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/readme</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
