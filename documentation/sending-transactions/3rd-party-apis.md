---
description: Trading APIs Triton hosts on your RPC endpoint for swaps, quotes, and bundle simulation.
---

# 3rd party APIs

Triton hosts third-party trading APIs on the same endpoint as your RPC, so you can quote, stream, and simulate without running extra infrastructure or staking any tokens. All three are included with an active Solana subscription, on both shared pools and dedicated nodes.

| API                                                | What it does                                                                                                                    | Use it when                                                                                              |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| [Metis swap API](3rd-party-apis/metis-swap-api.md) | Jupiter's routing and quote engine, self-hosted. Scans 20+ DEXes and AMMs and builds ready-to-sign swap transactions over HTTP. | You need swap quotes and routes: wallets, payment gateways, swap frontends, arbitrage bots.              |
| [Titan swap API](3rd-party-apis/titan-swap-api.md) | Streams swap quotes in real time over a WebSocket, using the Argos meta-aggregator and DART execution-time routing.             | You want live, low-latency pricing pushed to you: trading UIs, price dashboards, latency-sensitive bots. |
| [Jito bundles](3rd-party-apis/jito-bundles.md)     | Simulate transaction bundles on Triton's Jito-enabled RPC nodes.                                                                | You need to test a bundle's outcome before submitting it to Jito's Block Engine.                         |

It is common to combine them: use Metis or Titan to build and price a swap, and Jito bundle simulation to validate a bundle before you send it on to Jito.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
