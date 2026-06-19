# Metis swap API

Metis is Jupiter's routing and quote engine, self-hosted on Triton. You do not need any staked $JUP; Triton runs the binary for you. It scans 20+ DEXes and AMMs across Solana's DeFi ecosystem, finds the optimal route, splits trades when it helps, and builds ready-to-sign swap transactions.

## Key features

* **Liquidity aggregation**: combines liquidity from multiple AMMs and DEXes into a single route.
* **Platform fees**: set a fee wallet and pass a platform-fee parameter directly in the quote API.
* **ExactOut mode**: for payment flows where you need an exact output amount, for example accepting many tokens but settling in USDC.
* **Circular arbitrage**: Triton enables circular routes (USDC to token to USDC) that Jupiter's public API disables by default.

## Use cases

Metis on Triton can power:

* Wallets with simple swap functionality
* Payment gateways that accept many tokens and settle in a single asset
* Trading bots and arbitrage systems, including circular-arbitrage strategies
* DEX frontends with clean swap flows

## Getting started

Call the API on your Triton endpoint, under the `/metis` path.

```bash
curl 'https://<your-endpoint>.mainnet.rpcpool.com/<your-token>/metis/quote?\
inputMint=EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v&\
outputMint=So11111111111111111111111111111111111111112&\
amount=1000000&\
slippageBps=50'
```

## Executing a swap

After you get a quote from `/quote`:

1. **Get a quote**: call `/quote` as shown above.
2. **Get swap instructions**: pass the quote response to `/swap` or `/swap-instructions` to build the transaction.
3. **Sign and send**: sign with your wallet and submit to the network.

Metis builds the ready-to-sign transaction for you.

## Pricing

The self-hosted Metis swap API uses metered billing and is not rate-limited. It is $80 per million Metis queries. Because there are no rate limits, monitor your usage to avoid unexpected costs.

## Availability

Available to all customers with an active Solana subscription, on both shared RPC pools and dedicated nodes.

## API documentation

For the full parameter set and response shapes, see the [Metis docs](https://metis.builders/docs/get-quote). Background: [Metis API for Solana swaps is live on Triton](https://blog.triton.one/metis-api-for-solana-swaps-is-live-on-triton/).

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
