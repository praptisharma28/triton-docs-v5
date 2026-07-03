---
description: Keep real-time Solana streams reliable and ensure lowest-latency in production.
---

# Best practices

## Pick the right stream

* **Dragon's Mouth (gRPC), for lowest latency on a backend.** The ecosystem-standard gRPC stream: account and transaction updates pushed over a persistent connection as the node processes them, up to 400 ms faster than RPC polling. Server-to-server only.
* **Whirligig WebSocket, for browsers and frontends.** The same intra-slot latency as gRPC, but over a standard, browser-compatible Solana WebSocket, more reliable and faster than the native one, and with a full-transaction subscription the native API lacks. gRPC isn't available in browsers.
* **Fumarole, for reliability-first pipelines** (accounting, analytics, indexing, compliance). Adds persistence, redundancy, and 4 days of replay on top of Dragon's Mouth, which trades completeness for latency.
* **Deshred, for the earliest possible signal** (arbitrage, market making, liquidations, HFT). Reconstructs transactions from shreds before execution, so no confirmation guarantee; pair it with the `transactions` stream when you need finality.

## Provision the subscriber

* **Provision 5 Gbps minimum for large subscriptions, 10 Gbps for full-chain** (accounts plus transactions). The full feed can spike to \~1.3 to 1.8 Gbps, and cloud instances are often capped at \~1 Gbps by default.
* **Run subscribers on a persistent server (VPS or bare metal), not serverless functions** like Lambda or Cloud Functions. They recycle every few minutes, and each cold start and reconnect handshake lets the chain move on, so you drop messages and miss confirmations.
* **Update your `@triton-one/yellowstone-grpc` client to v5+ for high-volume streams.** We added NAPI-as-an-Engine (NaaE), which moves the gRPC engine into Rust for \~400% more throughput than the previous pure-JavaScript version. It's a drop-in replacement. [Learn more](https://blog.triton.one/grpc-js-alternative-napi-rust/).
* **Solve frequent disconnects early and rule out a client-side problem** (a weak or under-provisioned setup) before assuming a server fault. [Learn more](https://blog.triton.one/solana-grpc-streaming-optimisation-and-troubleshooting-2026-guide/).

## Stay on the tip during high load

Solana throughput keeps climbing, so a full-chain feed pushes more data every month. When your client cannot keep up, data backs up in buffers, you drift from the tip, and the server drops the connection. Most drift is client-side, and is usually caused by one of these:

* **The flow-control window is smaller than your bandwidth-delay product (BDP).** gRPC runs on HTTP/2, which only lets the server send a window's worth of data before it waits for an acknowledgement. The volume you need in flight is the BDP, `bandwidth × RTT` (2 Gbps at 10 ms RTT is \~2.38 MiB). A window below the BDP leaves bandwidth idle. Let the client and server negotiate it with adaptive window sizing; there is no reason to turn it off:

  ```rust
  GeyserGrpcBuilder::from_shared("https://your-endpoint")
      .http2_adaptive_window_size(true)
  ```

* **Large feeds run uncompressed.** Uncompressed full-chain traffic needs a larger BDP and is more sensitive to loss and jitter. Enable Zstd above \~8 ms RTT, and almost always above 30 ms:

  ```rust
  GeyserGrpcBuilder::from_shared("https://your-endpoint")
      .accept_compressed(Some(CompressionEncoding::Zstd))
  ```

* **You are too far from the endpoint.** Distance is the biggest driver of RTT. `yellowstone-grpc` caps the HTTP/2 window at 14.6 MiB, which puts the ceiling for full-chain streaming near 14.6 MiB / 2 Gbps, \~60 ms, so target under 50 ms. Measure yours with `ping <endpoint>` and run close to a Solana cluster:

  | Scenario | Typical distance | Expected RTT |
  | --- | --- | --- |
  | Same city or metro | under 50 km | 1 to 5 ms |
  | Regional, two data centres | 200 to 500 km | 5 to 15 ms |
  | Cross-country (US) | ~4,500 km | 60 to 80 ms |
  | Transatlantic | ~6,000 km | 70 to 100 ms |
  | Transpacific | ~9,000+ km | 140 to 200 ms |

* **You process inside the receive loop.** If one thread receives a message, parses it, writes it to a database, and only then reads the next, it fills buffers and disconnects under load. Decouple ingestion from processing: the receive loop should only push each message onto a queue or channel, and a worker pool handles parsing and writes.

Verify with the `client-ubuntu` test client, using the exact flags and filters you will run in production. A ping every ~10 seconds means you are holding the tip; a ping every 12 seconds or more, or none at all, means you are falling behind, so narrow the subscription or fix the causes above:

```bash
./client-ubuntu-22.04 \
  --http2-adaptive-window true \
  --compression zstd \
  --endpoint https://<your-endpoint>.rpcpool.com \
  --x-token <your-token> \
  subscribe --transactions --accounts --stats
```

## Multiplex and filter

* **Use one bi-directional stream and modify subscriptions in place** rather than opening new connections. A new subscribe request entirely overwrites the previous one, so **keep a local register of your full subscription config.**
* **Filter precisely:** subscription fields are logical AND and array values logical OR; include or exclude vote and failed transactions.
* **Trim payloads with `accounts_data_slice`** to receive only the bytes you need.
* **For blocks, exclude transactions or scope to specific accounts**, or use the block-meta subscription when you only need block-processed notifications.

## Handle commitment client-side

* **For maximum performance, manage `confirmed`/`finalized` client-side** rather than asking the server to buffer. Stream at `processed`, do the work early, and commit once you observe the commitment.
* **Buffer events by slot and release on slot notifications** (every event carries a slot; the slot notification arrives after that slot's events). When you see a `finalized` slot, retroactively mark its ancestors finalized too.

## Reconnect and replay

* **Recover from short disconnects with `from_slot`:** track your last processed slot, then reconnect and resubscribe from it. The server replays buffered updates, then continues live.
* **Deduplicate replayed updates** (replay starts at a slot boundary, so the boundary slot may repeat).
* **Check the earliest replayable slot first** via `SubscribeReplayInfo` (`first_available`); if `from_slot` is too old the request fails, so fall back to a fresh live subscription or your own backfill path.
* **Use the Rust client's built-in auto-reconnect (v13.1.0+)** for transparent reconnect, replay, and dedup. It is off by default.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
