---
description: >-
  Subscribe to transactions over Whirligig WebSockets. A Triton extension to the
  Solana WebSocket API.
---

# transactionSubscribe

A Triton extension: the standard Solana WebSocket API does not support transaction subscriptions. Stream transactions matching a filter over Whirligig, connecting with `wss://`.

The method takes two arguments: a `filter` (all fields optional) and a `config`.

## Subscribe

{% tabs %}
{% tab title="WebSocket message" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "transactionSubscribe",
  "params": [
    {
      "vote": false,
      "failed": false,
      "accounts": {
        "include": ["5qRKrGPYaQmazdtMnia1iC3gFbpgW4CT7KPrHq9nJGm5"]
      }
    },
    {
      "commitment": "processed",
      "encoding": "base58",
      "transactionDetails": "full",
      "showRewards": false,
      "maxSupportedTransactionVersion": 1
    }
  ]
}
```
{% endtab %}
{% endtabs %}

Connect to `wss://<your-endpoint>.mainnet.rpcpool.com/<your-token>`.

## Filter (first argument)

| Field               | Description                                             |
| ------------------- | ------------------------------------------------------- |
| `vote`              | Include vote transactions.                              |
| `failed`            | Include failed transactions.                            |
| `signature`         | Match a single transaction signature.                   |
| `accounts.include`  | Match transactions touching any of these accounts.      |
| `accounts.exclude`  | Exclude transactions touching any of these accounts.    |
| `accounts.required` | Only match transactions touching all of these accounts. |

## Config (second argument)

Equal to Solana's [`RpcBlockSubscribeConfig`](https://docs.rs/solana-client/latest/solana_client/rpc_config/struct.RpcBlockSubscribeConfig.html): `commitment`, `encoding`, `transactionDetails`, `showRewards`, and `maxSupportedTransactionVersion`.

## Notification

Updates arrive as `transactionNotification` messages, each with a `context` (slot, apiVersion) and a `value` carrying the slot, signature, and the full transaction with its meta. Unsubscribe with [transactionUnsubscribe](transactionunsubscribe.md).
