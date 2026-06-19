---
description: Subscribe to a stream of Sui event
---

# suix\_subscribeEvent

> A WebSocket subscription. Connect with `wss://` and send this as a WebSocket message; updates stream back until you unsubscribe.

## Request

{% tabs %}
{% tab title="WebSocket message" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_subscribeEvent",
  "params": [
    "<filter>"
  ]
}
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type        | Required | Description                                                                                                                                |
| --------- | ----------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `filter`  | EventFilter | Yes      | The filter criteria of the event stream. See [Event filter](https://docs.sui.io/build/event_api#event-filters) documentation for examples. |

## Response

Returns `SuiEvent`. (`Event`)
