---
description: Triton's transaction-sending tools and swap APIs.
layout:
  pagination:
    visible: false
---

# Sending transactions

You build and sign a transaction, then submit it so a validator includes it in a block. Landing reliably under load comes down to a few things:

* **Delivery.** Reaching the current leader quickly, with stake-weighted quality of service (SWQoS) so your transaction is prioritised.
* **Fees.** Attaching a priority fee that reflects the real market rate.
* **Protection.** Optionally restricting which validators can process your transaction, to avoid harmful MEV.

Triton covers each of these while keeping standard Solana behaviour where you want it.

## Pick your send path

Most transactions go through a Triton endpoint, where **Jet sender**, Triton's production sending engine, applies SWQoS, tracks the leader schedule, and forwards your transaction to the leader's TPU. Jet runs on isolated infrastructure that does not compete with your RPC reads. You can submit two ways, with a third path (the self-hosted Jet TPU client) for full client-side control.

|                       |   `sendTransaction`   |             `/sendtx`             | Jet TPU client (self-hosted) |
| --------------------- | :-------------------: | :-------------------------------: | :----------------------: |
| Interface             |    Solana JSON-RPC    |             HTTP POST             |       Rust library       |
| Reaches the TPU via   | your endpoint (server-side) |       your endpoint (server-side)       |   your client (direct)   |
| Overhead              |   JSON-RPC envelope   | minimal, no envelope or preflight |   operating the client   |
| SWQoS                 |           ✓           |                 ✓                 |             ✓            |
| Client to run         |          none         |                none               |            yes           |
| Best for              |  broad compatibility  |       browsers, HFT backends      |   bots, custom routers   |

* **`sendTransaction`** is the standard Solana JSON-RPC method. SWQoS is applied and your transaction is delivered to the leader for you.
* **`/sendtx`** is a direct HTTP submission endpoint on your Triton endpoint. It takes the same delivery path as `sendTransaction`, but skips the JSON-RPC envelope, so there is no JSON parsing, no CORS preflight, and a smaller payload. Lower latency, and no RPC client library needed. The [quickstart](https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/quickstart) walks through it.
* **Jet TPU client** is for full client-side control: your own machine sends straight to validator TPUs over QUIC, with per-transaction callbacks, custom routing, and Shield enforcement in your code. It is the sending logic from Jet sender, Triton's production engine, as a standalone library.

## Fees

Triton extends `getRecentPrioritizationFees` with a `percentile` parameter, so you can price a fee against the real market rate instead of the minimum. See [Priority fees API](https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/priority-fees-api).

## Protection

Create an on-chain allowlist or blocklist of validators and attach it when you send, so your transaction only goes to validators you trust. Apply it through Triton RPC (SWQoS) or the Jet TPU client. See [Shield MEV protection](https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/shield-mev-protection).

## Trading APIs

Triton hosts routing, quote, and bundle APIs so you do not assemble swap routes or wire up Jito yourself.

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-code-branch">:code-branch:</i> <strong>Metis swap API</strong></td><td>Self-hosted Jupiter routing and quotes across 20+ DEXes. No staked JUP required.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/3rd-party-apis/metis-swap-api">Metis swap API</a></td></tr><tr><td><i class="fa-route">:route:</i> <strong>Titan swap API</strong></td><td>Real-time streaming quotes over WebSocket from the Argos meta-aggregator.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/3rd-party-apis/titan-swap-api">Titan swap API</a></td></tr><tr><td><i class="fa-box">:box:</i> <strong>Jito bundles</strong></td><td>Simulate atomic bundles on Triton's Jito-enabled RPC. Submitting goes direct to the Jito Block Engine.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/3rd-party-apis/jito-bundles">Jito bundles</a></td></tr></tbody></table>

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-play">:play:</i> <strong>Quickstart</strong></td><td>Send a transaction with standard sendTransaction and with the Jet TPU client.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/quickstart">Quickstart</a></td></tr><tr><td><i class="fa-list-check">:list-check:</i> <strong>Best practices</strong></td><td>Land transactions reliably under congestion.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/best-practices">Best practices</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
