---
description: Stream or poll Solana swap quotes in real time and execute them, powered by Argos and DART.
---

# Titan Swap API

Titan is a plug-and-play quoting and swap execution layer for your transactions, hosted on Triton: it finds the optimal route across Solana liquidity and re-optimises your trade on-chain the moment it settles.

## Use cases

Titan is a good fit for:

* Trading interfaces that need live price updates
* Price-monitoring dashboards
* Trading bots that need low-latency price feeds
* Wallets with live swap pricing

## Features and benefits

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-layer-group">:layer-group:</i> <strong>Meta-aggregation</strong></td><td>The Argos engine compares and simulates its own routes against external aggregators and RFQ market makers (Hashflow, Pyth Express Relay), acting as your broker to return the best executable route.</td><td></td></tr><tr><td><i class="fa-route">:route:</i> <strong>DART on-chain routing</strong></td><td>Dynamically Allocated Real-Time routing finalises the trade at execution, re-optimising how your volume splits across pools against live state. Live on selected pairs.</td><td></td></tr><tr><td><i class="fa-tower-broadcast">:tower-broadcast:</i> <strong>Live quote streaming</strong></td><td>A single WebSocket channel pushes continuous quotes, each with route data and a ready-to-sign transaction.</td><td></td></tr><tr><td><i class="fa-coins">:coins:</i> <strong>Bandwidth-only pricing</strong></td><td>Billed at $0.08 / GB, with no per-request charge, rate or connection limits, or minimum spend.</td><td></td></tr></tbody></table>

## Getting started

Titan is enabled by default on every Triton Solana subscription. Depending on whether you poll for quotes or stream them, you use the HTTP or the WebSocket endpoint (find them in your [customer dashboard](https://customers.triton.one)) with the `/titan` path added:

* **Stream** (recommended): connect over `wss://` and receive continuous quote updates.
* **Poll**: request quotes over `https://` on the same `/titan` path.

```
wss://<your-endpoint>.mainnet.rpcpool.com/<your-token>/titan/api/v1/ws
```

### Example connection

```javascript
const ws = new WebSocket(
  "wss://<your-endpoint>.mainnet.rpcpool.com/<your-token>/titan/api/v1/ws"
);

ws.onopen = () => {
  console.log("Connected to the Titan Swap API");
};

ws.onmessage = (event) => {
  const data = JSON.parse(event.data);
  console.log("Price update:", data);
};
```

### Executing a swap

Each streamed quote carries the route information you need to execute:

1. **Connect**: open the WebSocket as shown above.
2. **Request quotes**: send a `NewSwapQuoteStream` request with your swap parameters.
3. **Receive the stream**: get continuous updates with the best routes across providers.
4. **Select a quote**: pick the best by your criteria, such as price or provider.
5. **Execute**: each `SwapRoute` arrives with either instructions to build and sign a transaction, or a ready-to-sign transaction you can submit directly.

For the request and response format and the full execution flow, see the [Titan Swap API docs](https://titan-exchange.gitbook.io/titan/developer-doc/dart-swap-api/overview). A TypeScript SDK is available: [@titanexchange/sdk-ts](https://www.npmjs.com/package/@titanexchange/sdk-ts).

## Pricing

Titan API is billed for bandwidth only (even on polls), at $0.08 per GB.


***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
