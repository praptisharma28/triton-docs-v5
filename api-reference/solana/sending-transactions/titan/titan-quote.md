---
description: Stream real-time swap quotes over the Titan Swap API WebSocket.
---

# Titan /quote

Titan streams swap quotes in real time over a WebSocket, instead of REST polling. The Argos meta-aggregator sources from multiple DEX aggregators and RFQ venues, and DART (Dynamically Allocated Real-Time routing) re-optimises the trade at execution time. Connect over `wss://`, not `https://`.

## Connect

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const ws = new WebSocket("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>/titan/api/v1/ws");

ws.onopen = () => {
  // subscribe to quote updates for your pair
};
ws.onmessage = (event) => {
  const quote = JSON.parse(event.data);
  console.log("quote:", quote);
};
```
{% endtab %}
{% endtabs %}

For the full message protocol (subscribe messages, quote payloads, DART routing fields) and the TypeScript SDK, see the [Titan DART Swap API docs](https://titan-exchange.gitbook.io/titan/developer-doc/dart-swap-api/overview).
