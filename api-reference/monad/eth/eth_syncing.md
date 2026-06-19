---
description: Returns an object with data about the sync status or false.
---

# eth\_syncing

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_syncing",
  "params": []
}'
```
{% endtab %}
{% endtabs %}

## Response

Returns `Syncing status`.

```json
{
  "startingBlock": "0x0",
  "currentBlock": "0x1518",
  "highestBlock": "0x9567a3"
}
```
