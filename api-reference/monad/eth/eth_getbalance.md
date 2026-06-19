---
description: Returns the balance of the account of given address.
---

# eth\_getBalance

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_getBalance",
  "params": [
    "0xfe3b557e8fb62b89f4916b721be55ceb828dbd73",
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

Returns `Balance`.

```json
"0x1cfe56f3795885980000"
```
