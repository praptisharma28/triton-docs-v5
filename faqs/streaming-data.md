---
description: >-
  Questions about gRPC streaming, WebSocket connections, and the Yellowstone
  suite
---

# Real-time streaming

## Project Yellowstone

<details>

<summary>Do you support devnet?</summary>

Yes. Triton offers devnet endpoints alongside mainnet, with Geyser gRPC and WebSocket streaming available on them. Contact support from your [customer dashboard](https://customers.triton.one) to set up a devnet endpoint.

</details>

<details>

<summary>How can I install Yellowstone on my server?</summary>

You don't need to install anything. All Geyser streaming products (Dragon's Mouth, Whirligig) are pre-installed on Triton infrastructure and enabled automatically for all subscriptions.

</details>

<details>

<summary>What is the maximum number of accounts I can subscribe to using Yellowstone gRPC?</summary>

For shared plans, the current limit is 200,000 subscriptions per IP. See [Rate and connection limits](https://app.gitbook.com/s/ACym6ZbIwDBDKhyKgDGy/rate-and-connection-limits) for connection caps and per-pool detail.

For dedicated nodes, there's no set limit. However, opening too many subscriptions for your servers to ingest can cause backpressure from your backend, leading to poor streaming performance. Please check that your server can handle the amount of data you are subscribing to.

</details>

<details>

<summary>What is Cloudbreak, and how does it improve latency?</summary>

Cloudbreak is Triton's account indexing engine that delivers `getProgramAccounts`, `getTokenAccountsByOwner`, and `getTokenAccountsByDelegate` **99%+ faster** by serving them from tailored PostgreSQL indexes instead of full account scans. Indexes are built automatically from your query traffic. Available on every Triton plan at standard RPC rates, no premium for indexed reads. See [Cloudbreak](https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/cloudbreak-indexed-accounts).

</details>

<details>

<summary>What is Fumarole, and how can I get access?</summary>

Fumarole is a streaming system designed for reliable, scalable streaming of account and transaction data. It aggregates data from multiple Solana nodes for high availability and persistence, with up to 4 days of retained stream history. Available on every Triton subscription, with regional endpoints (`ams.rpcpool.com` for Europe, `nyc.rpcpool.com` for the US). See [Fumarole](https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/fumarole-persistent-streams).

</details>

## gRPC and WebSocket

<details>

<summary>Can I use gRPC from a web browser?</summary>

No, gRPC is not supported in web browsers due to HTTP/2 and protobuf limitations. It is designed for backend applications. For frontend use, use Whirligig WebSocket instead.

</details>

<details>

<summary>Do endpoints support WebSocket connections?</summary>

Yes, on both shared and dedicated infrastructure. Your existing WebSocket connections automatically route through Whirligig WebSocket, our gRPC-backed WebSocket layer that delivers intra-slot account updates at `processed`, full `blockSubscribe` and `transactionSubscribe`, and significantly higher subscription limits than native pubsub. No code changes required.

</details>

<details>

<summary>How do I connect to WebSockets?</summary>

Replace `https` with `wss` in your RPC endpoint URL. For example: `wss://<your-endpoint>.rpcpool.com`. For backends, we recommend using gRPC instead, see [Dragon’s Mouth gRPC](https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/dragon-s-mouth-grpc) for the lower-latency path.

</details>

<details>

<summary>How can I use Geyser plugins?</summary>

Geyser plugins can be enabled on our dedicated Solana nodes. They provide real-time streams of on-chain data. Contact our support team to discuss your specific needs and we can configure a node for you.

</details>

<details>

<summary>I keep getting errors on my gRPC subscription. How can I check if the issue is with my endpoint or my code?</summary>

Run our prebuilt `client-ubuntu` test client. It's a small Linux CLI we ship for exactly this purpose, known-good code that hits your endpoint with the same gRPC subscription patterns your app does. If it errors, the issue is on the endpoint and we should look at it. If it streams cleanly, the issue is in your application code.

Full guide: [Verify your gRPC endpoint](https://app.gitbook.com/s/VeEf321LwceAVlSk9USV/solana/error-handling/verify-your-grpc-endpoint).

</details>

<details>

<summary>How do I get the earliest possible view of transactions?</summary>

Use Deshred. It streams transactions reconstructed from shreds before execution, so you see them earlier than a regular gRPC transaction stream. Built for latency-sensitive work like trading and arbitrage. See [Deshred Transactions gRPC](https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/deshred-transactions).

</details>

## Troubleshooting

<details>

<summary>My stream connects but no events are flowing</summary>

The most common cause is a filter that's too narrow. Try widening it (subscribe to all transactions or all token-program account writes) to confirm data is reaching you, then tighten back to what you actually need. Also check your commitment level: `processed` shows everything as soon as the validator sees it, while `confirmed` and `finalized` add buffering.

</details>

<details>

<summary>I'm getting 401 or 403 errors</summary>

Verify your endpoint URL and secret token from the [customer dashboard](https://customers.triton.one). For Whirligig, the token is part of the URL: `wss://<your-endpoint>.mainnet.rpcpool.com/<your-token>/whirligig`. For gRPC services, the token is passed as the `x-token` metadata header, not in the URL. If you've recently configured allowed origins, double-check your origin matches what's whitelisted.

</details>

<details>

<summary>My stream keeps disconnecting</summary>

Networks drop. Production clients should implement reconnect-and-resubscribe logic with exponential backoff. For Dragon's Mouth, you can resume from your last-seen slot using the `from_slot` field on `SubscribeRequest`. For Fumarole, reconnect with the same persistent-subscriber name and the cursor server-side picks up where you left off.

</details>

<details>

<summary>Events feel slower than expected</summary>

Three things to check. First, your commitment level: `processed` is fastest, `confirmed` and `finalized` add latency. Second, geographic distance: connect from a backend close to our streaming clusters. Third, your processing speed: if your event handler can't keep up, the server-side queue backs up and you fall behind.

</details>

<details>

<summary>Memory grows over time</summary>

Your handler is processing events slower than they arrive. Filter more aggressively to reduce volume, parallelise your handler across workers, or move heavy processing to a background queue and keep the stream loop minimal.

</details>

<details>

<summary>Do I need to send keepalive pings?</summary>

Sometimes. Some cloud providers (e.g. Cloudflare) close idle streams. If you see disconnections after periods of low traffic, send a periodic ping. Dragon's Mouth gRPC accepts a `ping` field on the subscribe request; Whirligig accepts `{"jsonrpc":"2.0","method":"ping"}`. The server returns a `pong` every 15 seconds, which you can use to confirm liveness.

</details>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
