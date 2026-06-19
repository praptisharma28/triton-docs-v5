---
description: Return the total coin balance for one coin type, owned by the address owner.
---

# suix\_getBalance

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_getBalance",
  "params": [
    "0xa6f5a7953a75dc632e696cabe60560522a017bf2fb0bd930d1ec22c06f1ee4e4",
    "0x168da5bf1f48dafc111b0a488fa454aca95e0b5e::usdc::USDC"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter   | Type       | Required | Description                                                                                                                                 |
| ----------- | ---------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `owner`     | SuiAddress | Yes      | the owner's Sui address                                                                                                                     |
| `coin_type` | string     | No       | optional type names for the coin (e.g., 0x168da5bf1f48dafc111b0a488fa454aca95e0b5e::usdc::USDC), default to 0x2::sui::SUI if not specified. |

## Response

Returns `Balance`.

```json
{
  "coinType": "0x168da5bf1f48dafc111b0a488fa454aca95e0b5e::usdc::USDC",
  "coinObjectCount": 15,
  "totalBalance": "15",
  "lockedBalance": {},
  "fundsInAddressBalance": "42"
}
```
