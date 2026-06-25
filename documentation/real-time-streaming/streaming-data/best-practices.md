---
description: Keep real-time Solana streams reliable and low-latency in production.
---

# Best practices

How to run real-time Solana streams reliably and at low latency.

## Pick the right stream

* **For lowest latency on a backend, use Dragon's Mouth (gRPC).** It is more stable than the legacy WebSocket interface and delivers intra-slot updates up to 400 ms faster. gRPC is server-to-server only.
* **For a browser or frontend, use Whirligig WebSockets.** It gives the same intra-slot advantage over a browser-compatible WebSocket; gRPC is not supported in browsers.
* **For "never miss an event" workloads (accounting, analytics, indexing, compliance, archival, or confirmed-data apps like wallets), use Fumarole** instead of raw gRPC. Dragon's Mouth prioritises latency over completeness; Fumarole adds persistence, redundancy, and replay.
* **For pre-parsed, program-specific data, use Program Data Streams (Vixen)** rather than building your own parsing.
* **For the earliest possible signal (arbitrage, market making, liquidations, HFT), use Deshred.** It reconstructs transactions from shreds before execution, so it has no confirmation guarantee; pair it with the normal `transactions` stream if you need finality.

## Provision the subscriber

* **Provision 5 Gbps minimum for large subscriptions, 10 Gbps for full-chain** (accounts plus transactions). The full feed can spike to \~1.3 to 1.8 Gbps, and cloud instances are often capped at \~1 Gbps by default.
* **Keep round-trip latency to the endpoint at 50 ms or less.**
* **Enable Zstd compression** and **set the HTTP/2 adaptive window to true.** Without compression it is hard to stay on the tip during spikes.
* **Run subscribers on a persistent server (VPS or bare metal), not serverless functions** like Lambda or Cloud Functions. They recycle every few minutes, and each cold start and reconnect handshake lets the chain move on, so you drop messages and miss confirmations.
* **On Node, use Triton's [`@triton-one/yellowstone-grpc`](https://www.npmjs.com/package/@triton-one/yellowstone-grpc) SDK (v5+), not the default `@grpc/grpc-js`.** Pure-JavaScript deserialisation blocks Node's single thread and triggers HTTP/2 backpressure, so you fall behind on heavy streams. The Triton SDK moves the gRPC engine into Rust via NAPI (their NaaE approach) for roughly 400% more throughput, as a drop-in replacement. Rust and Go clients are also good choices.
* **Benchmark with the `client-ubuntu` tool before going live:** a ping every 10 seconds at 60 to 80 Mbps means you can hold the tip without disconnects.
* **Treat frequent disconnects as a client-side problem** (weak or under-provisioned setup), not a server fault.

## Stay on the tip during high load

Solana throughput keeps climbing, so a full-chain feed pushes more data every month. When your client cannot keep up, data backs up in buffers, you drift from the tip, and the server drops the connection. Most drift is client-side, and is usually caused by one of these:

* **The flow-control window is smaller than your bandwidth-delay product (BDP).** gRPC runs on HTTP/2, which only lets the server send a window's worth of data before it waits for an acknowledgement. The volume you need in flight is the BDP, `bandwidth × RTT` (2 Gbps at 10 ms RTT is about 2.38 MiB). A window below the BDP leaves bandwidth idle. Let the client and server negotiate it with adaptive window sizing; there is no reason to turn it off:

  ```rust
  GeyserGrpcBuilder::from_shared("https://your-endpoint")
      .http2_adaptive_window_size(true)
  ```

* **Large feeds run uncompressed.** Uncompressed full-chain traffic needs a larger BDP and is more sensitive to loss and jitter. Enable Zstd above about 8 ms RTT, and almost always above 30 ms:

  ```rust
  GeyserGrpcBuilder::from_shared("https://your-endpoint")
      .accept_compressed(Some(CompressionEncoding::Zstd))
  ```

* **You are too far from the endpoint.** Distance is the biggest driver of RTT. `yellowstone-grpc` caps the HTTP/2 window at 14.6 MiB, which puts the ceiling for full-chain streaming near 14.6 MiB / 2 Gbps, about 60 ms, so target under 50 ms. Measure yours with `ping <endpoint>` and run close to a Solana cluster:

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
