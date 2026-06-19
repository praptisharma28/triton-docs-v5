---
description: Return the first four bytes of the chain's genesis checkpoint digest.
---

# sui\_getChainIdentifier

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_getChainIdentifier",
  "params": []
}'
```
{% endtab %}
{% endtabs %}

## Response

Returns `String`. (`string`)

```json
"4c78adac"
```
