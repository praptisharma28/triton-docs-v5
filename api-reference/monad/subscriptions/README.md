---
description: Push subscriptions over WebSockets, including Monad's speculative streams.
---

# Subscriptions

Push subscriptions over a WebSocket connection via `eth_subscribe`.

Subscribe to new block headers (`newHeads`) and `logs`, plus Monad's speculative `monadNewHeads` and `monadLogs` streams, which publish about one second earlier. Use these to react to chain updates without polling.
