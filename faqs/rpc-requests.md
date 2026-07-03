---
description: >-
  Questions about Triton's RPC services, latency, rate limits, scripts and
  monitoring
---

# RPC requests

## Performance and reliability

<details>

<summary>How reliable are Triton's RPC services?</summary>

Our infrastructure delivers 99.9% uptime, backed by multiple servers per region and automated health checks. This ensures that nodes stay synced and serve accurate, on-tip data.

If any node falls behind, requests are routed to a shared backup pool while the issue is resolved. Our infrastructure is monitored 24/7, and we're known for fast and responsive support.

</details>

<details>

<summary>How does Triton ensure minimal latency with RPC?</summary>

It's a big topic, but at a high level, we do it through:

* **Anycast:** routes traffic from anywhere to the nearest server
* **HTTP/3:** RPC over QUIC, with fewer connection round-trips and no head-of-line blocking, which especially helps clients connecting from outside our data centres.
* **Custom shred network:** our staked validators forward shreds straight to the RPC nodes, so they receive new block data among the first in Solana's fanout instead of last, where unstaked nodes normally sit.
* **Custom indexes:** Cloudbreak optimises `getProgramAccounts` (gPA) calls to improve query performance.

</details>

<details>

<summary>How does Triton handle network congestion on Solana?</summary>

We mitigate congestion with:

* **Staked nodes:** Prioritise transaction delivery to the block producer.
* **Load balancing:** Distribute traffic across 1000+ nodes.
* **Per-IP rate limiting:** Prevent abuse and ensure fair access.
* **Redundancy:** Automatic failover systems to maintain uptime during outages.

</details>

<details>

<summary>What happens if my RPC node receives excessive traffic? Will it affect transaction sending?</summary>

If traffic exceeds your node's capacity, it may lag behind the network's latest state, resulting in older data being served.

In such cases, requests will be rerouted to our backup pool, which supports upgrades and traffic spikes but introduces additional latency. If reliance on the backup pool becomes frequent, we will contact you to discuss scaling your deployment.

This doesn't affect transaction submission, as it's routed through a specialised Jet sending engine that isn't "noised" by reads.

</details>

<details>

<summary>What happens if I hit my rate limit?</summary>

If you've received `HTTP 429` error, pause requests for 10 seconds to clear the limit. We strongly recommend implementing a backoff-and-retry mechanism. See Handle 429 rate-limit errors for the pattern, or the [Rate and connection limits](https://app.gitbook.com/s/ACym6ZbIwDBDKhyKgDGy/get-started/rate-and-connection-limits) page for the budgets and headers.

</details>

<details>

<summary>Can I run scripts on the shared service?</summary>

Yes. Shared infrastructure handles backend workloads (scripts, trading bots, indexers) and is sized to absorb traffic spikes. If you expect sustained heavy load (heavy subscriptions, full-chain streams, custom Geyser configurations), a dedicated node might be a better fit.

</details>

<details>

<summary>How do I check my current rate limits?</summary>

Every endpoint exposes its live limits at `/ratelimits`. Run `curl https://<your-endpoint>/<your-token>/ratelimits`. Each JSON-RPC response also carries `X-Ratelimit-*` headers (limit, remaining, reset, plus the per-method pair), so you can back off before you hit a 429. See [Rate and connection limits](https://app.gitbook.com/s/ACym6ZbIwDBDKhyKgDGy/rate-and-connection-limits).

</details>

## Configuration and access

<details>

<summary>Do you support devnet?</summary>

Yes. Triton runs Solana mainnet, testnet, and devnet, with full compatibility with the standard JSON-RPC and WebSocket APIs across all three. Contact support from your [customer dashboard](https://customers.triton.one) to add a devnet endpoint.

</details>

<details>

<summary>How do I monitor my RPC usage?</summary>

Customers with dedicated nodes get a Grafana dashboard with real-time metrics: request volume, latency, error rates, and bandwidth usage.

For shared (pay-as-you-go) usage, open the **v3 Billing** tab in your [customer dashboard](https://customers.triton.one) to see total requests and GB used (the two dimensions you're billed on), plus a per-service breakdown.

</details>

<details>

<summary>Can I create new tokens or endpoints myself?</summary>

Accounts with standard permissions can't create tokens or endpoints directly. Contact support by clicking the chat icon in the bottom right of your [customer dashboard](https://customers.triton.one) to provision them.

If you need self-service, ask support to upgrade you to the operator role; once enabled, you can add tokens and endpoints from the dashboard yourself.

</details>

<details>

<summary>What's the __blocked.rpcpool.com origin on my endpoint?</summary>

`__blocked.rpcpool.com` is a system origin that Triton automatically adds to every endpoint. It signals that the endpoint only accepts requests authenticated by a valid token, blocking anonymous traffic.

Don't remove it. If you do and leave the allowed origins list empty, the endpoint becomes fully open: anyone can query it without a token, and the resulting traffic will be billed to your account.

</details>

<details>

<summary>Can I choose the geographic location of my RPC nodes?</summary>

Yes, dedicated node users can select primary regions (US, EU, or Asia Pacific) or request multi-region setups for redundancy and lower latency.

</details>

<details>

<summary>Are shared RPC subscriptions restricted to a single domain?</summary>

No. You can whitelist as many domains as you need from your customer dashboard.

</details>

<details>

<summary>Can Triton provide historical data?</summary>

Yes, shared infrastructure includes access to the complete ledger through Superbank pipeline. Dedicated node users can get archival nodes setup that store complete Solana history (e.g., past blocks, transactions). This requires additional storage (multi-TB scale) and is priced separately, reach out for a custom quote.

</details>

<details>

<summary>What is Jetstreamer?</summary>

Jetstreamer is the tool for bulk-backfilling large ranges of Solana history from the Old Faithful archive, with filtering and pluggable storage backends.

</details>

<details>

<summary>Can I use Jito sendBundle and bundle simulation with Triton RPCs?</summary>

Bundle simulation is available to everyone: see [Jito bundle simulation](https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/3rd-party-apis/jito-bundles) for the full reference.

`sendBundle` itself isn't routed through our infrastructure. Routing bundles through Triton would add an extra hop in front of the Jito block engine, which adds latency and runs counter to the reason you're using bundles in the first place.

The recommended pattern is to call the Jito block engine directly for sends, and use Triton for everything else (reads, streams, simulation).

</details>

<details>

<summary>Can I optimise for a specific region?</summary>

By default we use BGP Anycast to route every request to the nearest city automatically, requiring zero client-side setup. To pin your traffic to a specific region, contact support and we’ll set it up.

</details>

## Troubleshooting

<details>

<summary>I have an issue with my Web3.js socket connection. How can I fix it?</summary>

If you're hitting persistent socket or connection errors with `@solana/web3.js` (`fetch failed`, `Connect Timeout Error`, `ECONNREFUSED`, `ECONNRESET`, `other side closed`), see Web3.js socket and connection issues. It covers the root causes and the standard fixes.

</details>

<details>

<summary>How can I accurately measure latency when using Triton's RPC services?</summary>

To measure latency effectively, avoid relying on blockchain or explorer timestamps, as these may not reflect the actual performance of your RPC provider due to drift in the timestamping. The on-chain time and clock time drift apart, leading to misleading metrics.

Instead, record a timestamp immediately after receiving a transaction response from each provider in your application. This method captures the actual time it took for the request to complete, providing a reliable metric for assessing latency.

</details>

<details>

<summary>How can I monitor for delays on my gRPC connection?</summary>

To detect delays, examine the `slot` field included in all gRPC messages. The most precise method is to compare this slot value with a secondary source, such as another subscription or a `getSlot` call in your production environment. For additional validation, you may also cross-reference with an alternative RPC provider. Dedicated users can select the "Tracking Tip" folder in their Grafana Dashboard. The slot latency reflected here shows how much your node's slot latency differs from the rest of the network. As the node processes data from the network downstream, it sends this information through Geyser.

If you suspect your Geyser stream is experiencing a drastically different latency, check whether your receiving server is not providing back pressure to the RPC node due to bandwidth constraints.

</details>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
