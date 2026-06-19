---
description: Returns the number of most recent block.
---

# eth\_blockNumber

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_blockNumber",
  "params": []
}'
```
{% endtab %}
{% endtabs %}

## Response

Returns `Block number`.

```json
"0x2377"
```
