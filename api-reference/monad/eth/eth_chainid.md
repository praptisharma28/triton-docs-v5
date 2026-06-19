---
description: Returns the chain ID of the current network.
---

# eth\_chainId

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_chainId",
  "params": []
}'
```
{% endtab %}
{% endtabs %}

## Response

Returns `Chain ID`.

```json
"0x1"
```
