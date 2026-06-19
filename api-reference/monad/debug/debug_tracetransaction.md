---
description: Replay a transaction and return execution traces using the given tracer.
---

# debug\_traceTransaction

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "debug_traceTransaction",
  "params": [
    "0xdc0818cf78f21a8e70579cb46a43643f78291264dda342ae31049421c82d21ae",
    {
      "tracer": "callTracer"
    }
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type   | Required | Description                             |
| --------- | ------ | -------- | --------------------------------------- |
| `tx_hash` | string | Yes      | 32-byte transaction hash (hex-encoded). |
| `tracer`  | object | Yes      | Tracer configuration object.            |

## Response

```json
"0x..."
```
