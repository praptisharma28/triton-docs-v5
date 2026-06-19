---
description: >-
  Executes a new message call immediately without creating a transaction on the
  block chain.
---

# eth\_call

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_call",
  "params": [
    {
      "to": "0x69498dd54bd25aa0c886cf1f8b8ae0856d55ff13",
      "value": "0x1"
    },
    "latest"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter     | Type                   | Required | Description |
| ------------- | ---------------------- | -------- | ----------- |
| `Transaction` | GenericTransaction     | Yes      |             |
| `Block`       | BlockNumberOrTagOrHash | No       |             |

## Response

Returns `Return data`.

```json
"0x"
```
