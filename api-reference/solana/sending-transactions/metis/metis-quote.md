---
description: >-
  Get a swap route and quote from the Metis Swap API, Triton's hosted
  Jupiter routing engine.
---

# Metis /quote

Metis is Triton's hosted quoting and routing engine (no staked $JUP required). The `/metis/quote` endpoint scans 20+ DEXes and AMMs, finds the optimal route, splits trades when it helps, and returns a quote ready to turn into a signed transaction. Triton also enables circular arbitrage routes (USDC to token to USDC), which Jupiter's public API disables by default.

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl 'https://<your-endpoint>.mainnet.rpcpool.com/<your-token>/metis/quote?inputMint=EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v&outputMint=So11111111111111111111111111111111111111112&amount=1000000&slippageBps=50'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter     | Description                                      |
| ------------- | ------------------------------------------------ |
| `inputMint`   | Mint address of the input token.                 |
| `outputMint`  | Mint address of the output token.                |
| `amount`      | Amount in the input token's smallest units.      |
| `slippageBps` | Allowed slippage, in basis points (`50` = 0.5%). |

Metis exposes the full Jupiter Swap API surface (ExactOut mode, platform fees, `restrictIntermediateTokens`, and more). For the complete parameter set and response shape, see the [Jupiter swap reference](https://dev.jup.ag/docs/swap).

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
