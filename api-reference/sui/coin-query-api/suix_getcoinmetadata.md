---
description: >-
  Return metadata (e.g., symbol, decimals) for a coin. Note that if the coin's
  metadata was wrapped in the transaction that published its marker type, or the
  latest version of the metadata object is wra
---

# suix\_getCoinMetadata

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_getCoinMetadata",
  "params": [
    "0x168da5bf1f48dafc111b0a488fa454aca95e0b5e::usdc::USDC"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter   | Type   | Required | Description                                                                           |
| ----------- | ------ | -------- | ------------------------------------------------------------------------------------- |
| `coin_type` | string | Yes      | type name for the coin (e.g., 0x168da5bf1f48dafc111b0a488fa454aca95e0b5e::usdc::USDC) |

## Response

Returns `SuiCoinMetadata`.

```json
{
  "decimals": 9,
  "name": "Usdc",
  "symbol": "USDC",
  "description": "Stable coin.",
  "iconUrl": null,
  "id": "0x51ceab2edc89f74730e683ebee65578cb3bc9237ba6fca019438a9737cf156ae"
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
