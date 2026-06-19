---
description: Returns the value from a storage position at a given address.
---

# eth\_getStorageAt

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_getStorageAt",
  "params": [
    "0xfe3b557e8fb62b89f4916b721be55ceb828dbd73",
    "0x0",
    "latest"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter      | Type                   | Required | Description |
| -------------- | ---------------------- | -------- | ----------- |
| `Address`      | address                | Yes      |             |
| `Storage slot` | bytesMax32             | Yes      |             |
| `Block`        | BlockNumberOrTagOrHash | Yes      |             |

## Response

Returns `Value`.

```json
"0x0000000000000000000000000000000000000000000000000000000000000000"
```
