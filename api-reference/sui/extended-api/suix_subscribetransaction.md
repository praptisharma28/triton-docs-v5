---
description: Subscribe to a stream of Sui transaction effects
---

# suix\_subscribeTransaction

> A WebSocket subscription. Connect with `wss://` and send this as a WebSocket message; updates stream back until you unsubscribe.

## Request

{% tabs %}
{% tab title="WebSocket message" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_subscribeTransaction",
  "params": [
    "<filter>"
  ]
}
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type              | Required | Description |
| --------- | ----------------- | -------- | ----------- |
| `filter`  | TransactionFilter | Yes      |             |

## Response

Returns `SuiTransactionBlockEffects`. (`TransactionBlockEffects`)
