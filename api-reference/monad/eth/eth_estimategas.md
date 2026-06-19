---
description: >-
  Generates and returns an estimate of how much gas is necessary to allow the
  transaction to complete.
---

# eth\_estimateGas

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_estimateGas",
  "params": [
    {
      "from": "0xfe3b557e8fb62b89f4916b721be55ceb828dbd73",
      "to": "0x44aa93095d6749a706051658b970b941c72c1d53",
      "value": "0x1"
    }
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter     | Type               | Required | Description |
| ------------- | ------------------ | -------- | ----------- |
| `Transaction` | GenericTransaction | Yes      |             |
| `Block`       | BlockNumberOrTag   | No       |             |

## Response

Returns `Gas used`.

```json
"0x5208"
```
