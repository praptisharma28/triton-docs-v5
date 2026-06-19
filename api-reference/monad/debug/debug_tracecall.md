---
description: >-
  Execute a call against a block and return execution traces using the given
  tracer.
---

# debug\_traceCall

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "debug_traceCall",
  "params": [
    {
      "to": "0x...",
      "data": "0x..."
    },
    "latest",
    {
      "tracer": "callTracer",
      "tracerConfig": {
        "onlyTopCall": false
      }
    }
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter     | Type   | Required | Description                      |
| ------------- | ------ | -------- | -------------------------------- |
| `transaction` | object | Yes      | Transaction call object.         |
| `block`       | string | Yes      | Block tag or 32-byte block hash. |
| `tracer`      | object | Yes      | Tracer configuration object.     |

## Response

```json
"0x..."
```
