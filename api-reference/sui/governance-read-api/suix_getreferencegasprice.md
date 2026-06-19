---
description: Return the reference gas price for the network
---

# suix\_getReferenceGasPrice

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_getReferenceGasPrice",
  "params": []
}'
```
{% endtab %}
{% endtabs %}

## Response

Returns `BigInt<u64>`. (`BigInt_for_uint64`)

```json
1000
```
