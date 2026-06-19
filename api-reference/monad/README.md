---
description: >-
  Ethereum-compatible eth_* JSON-RPC, the debug namespace, Monad extensions, and
  speculative subscriptions.
---

# Overview

Monad is EVM-compatible, so Triton serves the standard Ethereum JSON-RPC plus Monad's own extensions. Error codes aim to match Ethereum's; some methods behave differently because of Monad's architecture.

## Endpoint and auth

```
https://<your-monad-endpoint>.rpcpool.com/<your-token>
```

Find your exact endpoint in the [customer dashboard](https://customers.triton.one). Authenticate with the token in the URL path or the `x-token` header.

## Method groups

* **Ethereum JSON-RPC**: the standard `eth_*`, `net_*`, and `web3_*` methods.
* **Subscriptions**: `eth_subscribe` over WebSockets, including Monad's speculative `monadNewHeads` and `monadLogs`.
* **Debug & tracing**: the `debug_*` namespace.
* **Monad extensions**: `eth_sendRawTransactionSync`, `admin_ethCallStatistics`, and the `txpool_*` methods.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
