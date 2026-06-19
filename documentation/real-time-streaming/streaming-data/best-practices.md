---
description: Keep real-time Solana streams reliable and low-latency in production.
---

# Best practices

How to run real-time Solana streams reliably and at low latency.

## Pick the right stream

* **For lowest latency on a backend, use Dragon's Mouth (gRPC).** It is more stable than the legacy WebSocket interface and delivers intra-slot updates up to 400 ms faster. gRPC is server-to-server only.
* **For a browser or frontend, use Whirligig WebSockets.** It gives the same intra-slot advantage over a browser-compatible WebSocket; gRPC is not supported in browsers.
* **For "never miss an event" workloads (accounting, analytics, indexing, compliance), use Fumarole** instead of raw gRPC. Dragon's Mouth prioritises latency over completeness; Fumarole adds persistence, redundancy, and replay.
* **For pre-parsed, program-specific data, use Program Data Streams (Vixen)** rather than building your own parsing.

## Provision the subscriber

* **Provision at least 5 Gbps** for large or full-chain subscriptions. The full feed can spike to \~1.3 to 1.8 Gbps, and cloud instances are often capped at 1 Gbps by default.
* **Keep round-trip latency to the endpoint at 50 ms or less.**
* **Enable Zstd compression** and **set the HTTP/2 adaptive window to true.** Without compression it is hard to stay on the tip during spikes.
* **Don't run subscribers on serverless / Lambdas** (they leave many open connections and degrade service) and **don't use vanilla NodeJS** (too slow; use Rust, Golang, or the special NodeJS/TypeScript client).
* **Benchmark with the `client-ubuntu` tool before going live:** a ping every 10 seconds at 60 to 80 Mbps means you can hold the tip without disconnects.
* **Treat frequent disconnects as a client-side problem** (weak or under-provisioned setup), not a server fault.

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

## Use deshred only for the earliest signal

* **Deshred (beta) delivers transactions reconstructed from shreds before execution** — the earliest usable signal, for arbitrage, market making, copy trading, liquidations, and HFT.
* **Don't rely on it for execution results or finality.** It has no confirmation guarantee (a transaction may fail, fork, or never confirm); pair it with the normal `transactions` stream if you need those. `SubscribeDeshred` is a separate RPC.

## Fumarole reliability and failover

* **Use Fumarole for persistence and high availability:** it stores up to 4 days of state, lets you disconnect and resume from your last position, and merges multiple downstream nodes so a node restart does not interrupt your stream.
* **Connect Fumarole to a regional endpoint** (for example `ams.rpcpool.com`), not the shared `*.mainnet.rpcpool.com`. Persistent subscribers are stateful per cluster and do not replicate across regions, so GeoDNS on a shared endpoint can route you where your subscriber does not exist. Pick the region closest to your backend.
* **Implement cross-region failover client-side** by recreating the persistent subscriber from the last slot observed on the failing cluster. Triton does not synchronise subscriber state or orchestrate failover. Do the prep (durable last-consumed-slot tracking, idempotent processing, outage detection) during normal operation, and start the secondary at `last_slot + 1`. See the Fumarole cluster failover guide for the full procedure.

## Operate streaming nodes for reliability

* **Run Geyser on a dedicated node, separate from RPC.** Sharing risks RPC load putting the node behind, which makes Geyser fall behind and miss updates. For complete reliability, run two dedicated nodes (failover plus maintenance and version upgrades).

## Whirligig, Vixen, and auth specifics

* **Whirligig:** append `/whirligig` to your endpoint (some clients need a trailing slash), use the extended (non-standard) `transactionSubscribe` for filtered transaction streams, unsubscribe with the `id` returned by each subscribe, and ping to keep the connection alive (inactive over 60 seconds is closed).
* **Vixen:** open multiple subscriptions to stream different programs in parallel, connect to the closest region (USA/EU/AP), keep the SDK on the latest version to avoid parser errors, and share one Dragon's Mouth stream across pipelines.
* **Keep gRPC streams alive with periodic pings** behind idle-closing proxies (the server replies `pong` every 15 seconds).
* **Authenticate with the `x-token` metadata header**, not a token in the URL (token-in-URL returns `403` on gRPC).
* **Don't read ZK Compression / Light Protocol accounts over gRPC** (excluded; \~10 MB blobs, gigabits/sec).
* **Run streaming subscribers on a dedicated node**, not the shared service.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
