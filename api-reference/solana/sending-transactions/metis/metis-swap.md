---
description: Build a signed-ready swap transaction from a Metis quote.
---

# Metis /swap

Turns a quote from [Metis quote](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/sending-transactions/metis/metis-quote) into a ready-to-sign swap transaction. Served at `/metis/swap` on your Triton endpoint. The flow is: get a quote, post it to `/metis/swap` to build the transaction, then sign and send it to the network.

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl 'https://<your-endpoint>.mainnet.rpcpool.com/<your-token>/metis/swap' -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "quoteResponse": { ... },
    "userPublicKey": "<your-wallet-pubkey>"
  }'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter       | Description                                       |
| --------------- | ------------------------------------------------- |
| `quoteResponse` | The full quote object returned by `/metis/quote`. |
| `userPublicKey` | The wallet that will sign and send the swap.      |

Metis mirrors the Jupiter swap surface (wrap/unwrap SOL, priority fees, platform fees, and more). For the complete request and response, see the [Jupiter swap reference](https://dev.jup.ag/docs/swap/build-swap-transaction).

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
