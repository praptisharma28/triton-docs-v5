---
description: Stream Solana swap quotes in real time and execute them, powered by Argos and DART.
---

# Titan swap API

The Titan swap API streams swap quotes in real time over a WebSocket. The Argos routing engine acts as a meta-aggregator, sourcing from multiple DEX aggregators and RFQ venues. Streaming over a WebSocket keeps latency low and lets a frontend update prices live without overwhelming the server with HTTP requests.

## Key features

* **Real-time streaming**: a WebSocket connection pushes continuous price updates.
* **Meta-aggregator**: sources from multiple DEX aggregators and RFQ venues to minimise slippage.
* **Low latency**: streams quotes faster than REST polling.
* **DART routing**: [Dynamically Allocated Real-Time routing](https://titan-exchange.gitbook.io/titan/developer-doc/dart-swap-api/overview) re-optimises a trade at the moment of execution, not just at quote time, for the best execution when it matters.

## Use cases

Titan is a good fit for:

* Trading interfaces that need live price updates
* Price-monitoring dashboards
* Trading bots that need low-latency price feeds
* Wallets with live swap pricing

## Getting started

Connect over `wss://` (not `https://`, this is a WebSocket service) on your Triton endpoint, under the `/titan` path.

```
wss://<your-endpoint>.mainnet.rpcpool.com/<your-token>/titan/api/v1/ws
```

### Example connection

```javascript
const ws = new WebSocket(
  "wss://<your-endpoint>.mainnet.rpcpool.com/<your-token>/titan/api/v1/ws"
);

ws.onopen = () => {
  console.log("Connected to the Titan swap API");
};

ws.onmessage = (event) => {
  const data = JSON.parse(event.data);
  console.log("Price update:", data);
};
```

## Executing a swap

Each streamed quote carries the route information you need to execute:

1. **Connect**: open the WebSocket as shown above.
2. **Request quotes**: send a `NewSwapQuoteStream` request with your swap parameters.
3. **Receive the stream**: get continuous updates with the best routes across providers.
4. **Select a quote**: pick the best by your criteria, such as price or provider.
5. **Execute**: each `SwapRoute` arrives with either instructions to build and sign a transaction, or a ready-to-sign transaction you can submit directly.

## Pricing

Streaming is billed at $0.08 per GB.

## API documentation

For the request and response format and the full execution flow, see the [Titan swap API docs](https://titan-exchange.gitbook.io/titan/titan-developer-docs/apis/swap-api#websocket-connections). A TypeScript SDK is available: [@titanexchange/sdk-ts](https://www.npmjs.com/package/@titanexchange/sdk-ts). Background: [Titan Prime API for Solana swaps is live on Triton](https://blog.triton.one/titan-prime-api-for-solana-swaps-is-live-on-triton/).

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
