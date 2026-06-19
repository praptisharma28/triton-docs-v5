---
description: Returns code at a given address.
---

# eth\_getCode

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_getCode",
  "params": [
    "0xa50a51c09a5c451c52bb714527e1974b686d8e77",
    "latest"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type                   | Required | Description |
| --------- | ---------------------- | -------- | ----------- |
| `Address` | address                | Yes      |             |
| `Block`   | BlockNumberOrTagOrHash | Yes      |             |

## Response

Returns `Bytecode`.

```json
"0x60806040526004361060485763ffffffff7c01...470a2ea70029"
```
