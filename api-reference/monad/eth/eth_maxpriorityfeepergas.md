---
description: Returns the current maxPriorityFeePerGas per gas in wei.
---

# eth\_maxPriorityFeePerGas

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_maxPriorityFeePerGas",
  "params": []
}'
```
{% endtab %}
{% endtabs %}

## Response

Returns `Max priority fee per gas`.

```json
"0x773c23ba"
```
