---
description: >-
  Open a push subscription over a WebSocket connection. Monad adds speculative
  monadNewHeads and monadLogs streams that publish about one second earlier.
---

# eth\_subscribe

Open a push subscription over a WebSocket connection (`wss://`). Monad supports the standard `newHeads` and `logs` subscriptions, plus two speculative Monad extensions that publish roughly one second earlier than their finalized counterparts.

## Request

{% tabs %}
{% tab title="WebSocket message" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_subscribe",
  "params": [
    "newHeads"
  ]
}
```
{% endtab %}
{% endtabs %}

Connect to `wss://<your-monad-endpoint>.rpcpool.com/<your-token>`.

## Subscription types

| Type            | Description                                                        |
| --------------- | ------------------------------------------------------------------ |
| `newHeads`      | New block headers as they are added to the chain.                  |
| `logs`          | Logs matching a filter object passed as the second parameter.      |
| `monadNewHeads` | Speculative new block headers, published about one second earlier. |
| `monadLogs`     | Speculative logs, published about one second earlier.              |

## Not supported

Monad does not support the `newPendingTransactions` or `syncing` subscriptions, nor EIP-4844 blob transactions.
