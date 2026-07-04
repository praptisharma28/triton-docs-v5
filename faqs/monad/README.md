---
description: >-
  Common questions about running on Monad: endpoints, methods, and how to
  connect.
---

# General

## Networks and access

<details>

<summary>Which Monad networks does Triton support?</summary>

Both Monad Mainnet (chain ID 143) and Testnet (chain ID 10143), each as a shared subscription. Triton also runs a Monad validator.

</details>

<details>

<summary>What does Triton offer on Monad?</summary>

JSON-RPC over HTTPS and WebSocket (WSS) endpoints, served from shared RPC pools. Monad is EVM-compatible, so the API is Geth-compatible. Triton does not offer gRPC streaming for Monad.

</details>

<details>

<summary>Does Triton offer webhooks, a Data API, or custom indexing for Monad?</summary>

No. On Monad, Triton provides standard JSON-RPC over HTTPS and WebSocket (WSS), plus a Triton-operated validator. There are no webhooks, Data API, or custom indexing.

</details>

<details>

<summary>I only see a testnet endpoint. How do I get Monad mainnet?</summary>

Both Monad Mainnet (chain ID 143) and Testnet (chain ID 10143) are available. Get your endpoint and token from your [customer dashboard](https://customers.triton.one). If a network is not showing, contact support via the dashboard chat to have it enabled.

</details>

<details>

<summary>How do I authenticate Monad requests?</summary>

Send your token in the `x-token` header, or in the URL path for HTTPS and WSS. The same token works across your Monad endpoints.

</details>

## Methods and streaming

<details>

<summary>Which methods are available?</summary>

Most Geth JSON-RPC methods (for example `eth_blockNumber`). The WebSocket API supports `eth_subscribe` (for example `newHeads` and `logs`). There are no unary calls over the WebSocket; subscriptions run over the WebSocket only. Monad's differences from Geth are documented at [docs.monad.xyz/reference/rpc-differences](https://docs.monad.xyz/reference/rpc-differences).

</details>

<details>

<summary>Can I stream Monad data?</summary>

Yes, over WebSocket using `eth_subscribe`. Triton does not offer gRPC for Monad.

</details>

## Getting started

<details>

<summary>How do I get started on Monad?</summary>

Sign up at [customers.triton.one](https://customers.triton.one), get your endpoint and token from the dashboard, then send JSON-RPC over HTTPS or subscribe over WebSocket. One account, and the same minimum deposit, covers Solana, Sui, and Monad.

</details>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
