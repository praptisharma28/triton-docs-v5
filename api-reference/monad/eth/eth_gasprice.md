---
description: Returns the current price per gas in wei.
---

# eth\_gasPrice

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_gasPrice",
  "params": []
}'
```
{% endtab %}
{% endtabs %}

## Response

Returns `Gas price`.

```json
"0x3e8"
```
