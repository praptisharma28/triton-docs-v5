---
description: >-
  Monad RPC on Triton: the Ethereum-compatible eth_* JSON-RPC, the debug
  namespace, Monad extensions, and speculative subscriptions.
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

For the canonical Monad reference and any chain-specific differences, see the [official Monad JSON-RPC reference](https://docs.monad.xyz/reference/json-rpc/).
