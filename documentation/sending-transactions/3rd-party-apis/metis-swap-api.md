---
description: Triton-hosted swap quoting and routing across 20+ DEXes, no staked JUP required.
---

# Metis swap API

Metis is the Triton-hosted Jupiter routing and quote engine: integrate swaps into your app over a plain HTTP endpoint, with no staked $JUP required. It scans 20+ DEXes and AMMs across Solana's DeFi ecosystem, finds the optimal route, splits trades when it helps, and builds ready-to-sign swap transactions.

## Use cases

Metis on Triton can power:

* Wallets with simple swap functionality
* Payment gateways that accept many tokens and settle in a single asset
* Trading bots and arbitrage systems, including circular-arbitrage strategies
* DEX frontends with clean swap flows

## Features and benefits

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-wallet">:wallet:</i> <strong>Platform fees</strong></td><td>Set a fee wallet and pass a platform-fee parameter directly in the quote API.</td><td></td></tr><tr><td><i class="fa-coins">:coins:</i> <strong>ExactOut mode</strong></td><td>For payment flows where you need an exact output amount, for example accepting many tokens but settling in USDC.</td><td></td></tr><tr><td><i class="fa-rotate">:rotate:</i> <strong>Circular arbitrage</strong></td><td>Triton enables circular routes (USDC to token to USDC) that Jupiter's public API disables by default.</td><td></td></tr><tr><td><i class="fa-gauge">:gauge:</i> <strong>No rate limits</strong></td><td>Metered billing with no rate limits on Metis queries.</td><td></td></tr></tbody></table>

## Getting started

Metis is enabled by default on every Triton Solana subscription. Use your Triton endpoint, from your [customer dashboard](https://customers.triton.one), and add the `/metis` path.

```bash
curl 'https://<your-endpoint>.mainnet.rpcpool.com/<your-token>/metis/quote?\
inputMint=EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v&\
outputMint=So11111111111111111111111111111111111111112&\
amount=1000000&\
slippageBps=50'
```

### Executing a swap

After you get a quote from `/quote`:

1. **Get a quote**: call `/quote` as shown above.
2. **Get swap instructions**: pass the quote response to `/swap` or `/swap-instructions` to build the transaction.
3. **Sign and send**: sign with your wallet and submit to the network.

Metis builds the ready-to-sign transaction for you.

## Pricing

The Metis swap API uses metered billing and is not rate-limited. It is `$0.08 / GB` of bandwidth plus `$80 / million` Metis queries. Because there are no rate limits, monitor your usage to avoid unexpected costs.

## API documentation

For the full parameter set and response shapes, see the [Metis docs](https://metis.builders/docs/get-quote). Background: [Metis API for Solana swaps is live on Triton](https://blog.triton.one/metis-api-for-solana-swaps-is-live-on-triton/).

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
