---
description: Return the validator APY
---

# suix\_getValidatorsApy

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_getValidatorsApy",
  "params": []
}'
```
{% endtab %}
{% endtabs %}

## Response

Returns `ValidatorApys`.

```json
{
  "apys": [
    {
      "address": "0x9d77e49d53f92bc8310f0ccc3257dcc85bada4a729d650f77622264321297809",
      "apy": 0.06
    },
    {
      "address": "0x27838b06db0346808ffb0676099de0408b31759f57b69c52e09410a66f9a23c3",
      "apy": 0.02
    },
    {
      "address": "0x4be9913b6697a5e83e02e2a0fc747057ba0901e4d9b1e04de75ea2699a441321",
      "apy": 0.05
    }
  ],
  "epoch": "420"
}
```
