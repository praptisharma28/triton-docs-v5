---
description: Return total supply for a coin
---

# suix\_getTotalSupply

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_getTotalSupply",
  "params": [
    "0xe5c651321915b06c81838c2e370109b554a448a78d3a56220f798398dde66eab::acoin::ACOIN"
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

Returns `Supply`.

```json
{
  "value": "12023692"
}
```
