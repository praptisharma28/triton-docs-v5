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
