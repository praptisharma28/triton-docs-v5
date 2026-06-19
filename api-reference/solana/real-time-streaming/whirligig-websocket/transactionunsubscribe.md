---
description: Cancel a transactionSubscribe subscription.
---

# transactionUnsubscribe

Cancel a [transactionSubscribe](transactionsubscribe.md) subscription using the subscription id returned when you subscribed.

{% tabs %}
{% tab title="WebSocket message" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "transactionUnsubscribe",
  "params": [0]
}
```
{% endtab %}
{% endtabs %}
