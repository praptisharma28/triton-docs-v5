---
description: Return statistics about eth_call execution on the node.
---

# admin\_ethCallStatistics

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "admin_ethCallStatistics",
  "params": []
}'
```
{% endtab %}
{% endtabs %}

## Response

```json
"0x..."
```
