---
description: Triton's transaction-sending tools, swap APIs, and SWQoS routing.
layout:
  pagination:
    visible: false
---

# Sending transactions

You build and sign a transaction, then submit it so a validator includes it in a block. Landing reliably under load comes down to a few things:

* **Fast delivery.** Reaching the current leader quickly through a specialised traffic route that does not compete with your read requests.
* **Stake-weighted quality of service (SWQoS).** Prioritising your transactions for higher landing rates and reliability, added by default on every Triton endpoint at no extra cost.
* **Priority fees.** Attaching a fee that reflects the real market rate without overpaying.
* **Protection.** Optionally restricting which validators can process your transaction, to avoid harmful MEV.


## Pick your send path

Every transaction sent to a Triton endpoint is handled by **Jet**, Triton's SWQoS-powered production sending engine: it tracks the leader schedule and forwards your transaction directly to the leader's TPU. You can submit through two managed routes depending on your use case, or self-host the Jet TPU client for full client-side control.

* **`sendTransaction`** is the standard Solana JSON-RPC method. We route it through our specialised Jet engine for the lowest latency and apply SWQoS for higher reliability.
* **`/sendtx`** is a direct HTTP submission endpoint that takes the same delivery path as `sendTransaction`, but skips the JSON-RPC envelope, so there is no JSON parsing, no CORS preflight, and a smaller payload. It delivers lower latency and needs no RPC client library.
* **[Jet TPU client](https://github.com/rpcpool/yellowstone-jet)** is an open-source Rust crate you self-host for full client-side control and absolute minimal latency: once you wire it up, it sends your transactions straight to validator TPUs over QUIC, with per-transaction callbacks, custom routing, and Shield integration out of the box.

|                       |   `sendTransaction`   |             `/sendtx`             | Jet TPU client (self-hosted) |
| --------------------- | :-------------------: | :-------------------------------: | :----------------------: |
| Interface             |    Solana JSON-RPC    |             HTTP POST             |       Rust library       |
| Reaches the TPU via   | your endpoint (server-side) |       your endpoint (server-side)       |   your client (direct)   |
| Latency               |          low          |    lower, no JSON-RPC overhead    | minimal, direct to the leader |
| Ease of use           |    any Solana SDK     | a single HTTP POST, no RPC library |   you operate the client   |
| SWQoS added by default (no extra charge) | ✓ | ✓ | ✗ (unless you integrate it) |
| `skipPreflight` option | ✓ | ✗ (always skips preflight) | ✗ (direct send, no preflight) |
| Client to run         |          none         |                none               |            yes           |
| Best for              |  broad compatibility  |       browsers, HFT backends      |   bots, custom routers   |

All of the methods can be combined with Triton's percentile-based priority fee estimation and Shield for MEV protection:

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-arrow-trend-up">:arrow-trend-up:</i> <strong>Priority fees API</strong></td><td>Percentile-based fee estimates that price against the real market rate, not the minimum. Triton extends <code>getRecentPrioritizationFees</code> with a <code>percentile</code> parameter.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/priority-fees-api">Priority fees API</a></td></tr><tr><td><i class="fa-shield">:shield:</i> <strong>Shield Protection</strong></td><td>Attach an on-chain validator allowlist or blocklist so only validators you trust process your transaction. Enforced via Triton RPC (SWQoS) or the Jet TPU client.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/shield-mev-protection">Shield Protection</a></td></tr></tbody></table>

## Trading APIs

Triton hosts routing, quote, and bundle APIs if you don't want to assemble any of these yourself.

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-code-branch">:code-branch:</i> <strong>Metis swap API</strong></td><td>Quoting, routing, and swaps across 20+ DEXes, hosted by Triton. No staked JUP required.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/3rd-party-apis/metis-swap-api">Metis swap API</a></td></tr><tr><td><i class="fa-route">:route:</i> <strong>Titan swap API</strong></td><td>Real-time streaming quotes over WebSocket from the Argos meta-aggregator.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/3rd-party-apis/titan-swap-api">Titan swap API</a></td></tr><tr><td><i class="fa-box">:box:</i> <strong>Jito bundle simulation</strong></td><td>Simulate atomic bundles on Triton's Jito-enabled RPC. Submitting goes direct to the Jito Block Engine.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/3rd-party-apis/jito-bundles">Jito bundle simulation</a></td></tr></tbody></table>

## Pricing

| What | Price |
| --- | --- |
| `sendTransaction`, `/sendtx` | Standard RPC: `$0.08 / GB` of bandwidth plus `$10 / million` requests |
| `simulateTransaction` | Its own standard RPC call, billed as a separate request |
| SWQoS | Included at no extra charge on every send through your Triton endpoint |
| Priority fees API | Standard RPC: `$0.08 / GB` plus `$10 / million` calls |
| Metis swap API | `$0.08 / GB` plus `$80 / million` queries |
| Titan swap API | Bandwidth only, `$0.08 / GB` |
| Jito bundle simulation | Simulation billed as standard RPC; bundle submission goes to the Jito Block Engine and is not billed by Triton |

With `skipPreflight: false` (the default), `sendTransaction` runs the preflight simulation as a separate call, so one send bills as 2 requests. `/sendtx` always skips preflight.

Each product page carries the details, and the full rate card is at [triton.one/pricing](https://triton.one/pricing).

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-play">:play:</i> <strong>Quickstart</strong></td><td>Send a transaction with standard sendTransaction and with the Jet TPU client.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/quickstart">Quickstart</a></td></tr><tr><td><i class="fa-list-check">:list-check:</i> <strong>Best practices</strong></td><td>Land transactions reliably under congestion.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/best-practices">Best practices</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
