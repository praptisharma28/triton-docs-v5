---
description: >-
  Replay a block by number or tag and return execution traces using the given
  tracer.
---

# debug\_traceBlockByNumber

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "debug_traceBlockByNumber",
  "params": [
    "latest",
    {
      "tracer": "callTracer"
    }
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter      | Type   | Required | Description                                                                   |
| -------------- | ------ | -------- | ----------------------------------------------------------------------------- |
| `block_number` | string | Yes      | Block number (hex) or tag: `latest`, `safe`, `finalized`, `pending`.          |
| `tracer`       | object | Yes      | Tracer configuration: `tracer` (string) and optional `tracerConfig` (object). |

## Response

```json
"0x..."
```
