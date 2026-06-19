---
description: Build a signed-ready swap transaction from a Metis quote.
---

# Metis /swap

Turns a quote from [Metis quote](metis-quote.md) into a ready-to-sign swap transaction. Served at `/metis/swap` on your Triton endpoint. The flow is: get a quote, post it to `/metis/swap` to build the transaction, then sign and send it to the network.

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
