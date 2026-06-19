---
description: Get token prices from the Metis Swap API, Triton's self-hosted Jupiter engine.
---

# Metis /price

Returns the current price for a token, derived from Metis routing. Use it to display prices without building a full swap. Served at `/metis/price` on your Triton endpoint.

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl 'https://<your-endpoint>.mainnet.rpcpool.com/<your-token>/metis/price?ids=So11111111111111111111111111111111111111112'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Description                              |
| --------- | ---------------------------------------- |
| `ids`     | Comma-separated mint addresses to price. |

Metis mirrors the Jupiter price surface. For the full parameter set and response shape, see the [Jupiter price reference](https://dev.jup.ag/docs/price-api).
