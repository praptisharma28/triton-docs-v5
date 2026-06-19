---
description: >-
  Returns the current network ID. This is usually equivalent to the chainID, but
  may differ from it for some legacy networks or special testnets.
---

# net\_version

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "net_version",
  "params": []
}'
```
{% endtab %}
{% endtabs %}

## Response

Returns `Network ID`.

```json
"1"
```
