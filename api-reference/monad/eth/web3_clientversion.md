---
description: Return the current client version.
---

# web3\_clientVersion

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "web3_clientVersion",
  "params": []
}'
```
{% endtab %}
{% endtabs %}

## Response

```json
"0x..."
```
