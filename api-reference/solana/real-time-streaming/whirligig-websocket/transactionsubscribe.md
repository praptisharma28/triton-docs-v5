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

The subscribe call returns a subscription id (`{"jsonrpc":"2.0","result":70527,"id":1}`). Updates then arrive as `transactionNotification` messages, each with a `context` (slot) and a `value` carrying the slot and the full transaction — the encoded transaction, its `meta`, and its `version`. Long fields are truncated (`...`) in the example below:

```json
{
  "jsonrpc": "2.0",
  "method": "transactionNotification",
  "params": {
    "result": {
      "context": { "slot": 430532825 },
      "value": {
        "slot": 430532825,
        "transaction": {
          "transaction": [
            "61zPSp4TNQGmAQjxuU2TDpJDSYC4vh1rjedsDQFX...M6nm5cnb",
            "base58"
          ],
          "meta": {
            "err": null,
            "status": { "Ok": null },
            "fee": 5000,
            "preBalances": [66412920, 2039280, 0, 2039280, "..."],
            "postBalances": [0, 0, 2039280, 2039280, "..."],
            "innerInstructions": [
              {
                "index": 2,
                "instructions": [
                  {
                    "programIdIndex": 12,
                    "accounts": [9],
                    "data": "84eT",
                    "stackHeight": 2
                  },
                  "..."
                ]
              }
            ],
            "logMessages": [
              "Program ComputeBudget111111111111111111111111111111 invoke [1]",
              "Program ComputeBudget111111111111111111111111111111 success",
              "..."
            ],
            "preTokenBalances": [
              {
                "accountIndex": 1,
                "mint": "Es9vMFrzaCERmJfrF4H2FYD4KCoNkY11McCe8BenwNYB",
                "uiTokenAmount": {
                  "uiAmount": 0.1379,
                  "decimals": 6,
                  "amount": "137900",
                  "uiAmountString": "0.1379"
                },
                "owner": "EnA3aCS9nYfYSniQBFV8G8QxPyYgZGXAs9tsuj5tSecR",
                "programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"
              },
              "..."
            ],
            "postTokenBalances": ["..."],
            "rewards": null,
            "loadedAddresses": { "writable": [], "readonly": [] },
            "computeUnitsConsumed": 16059,
            "costUnits": 18648
          },
          "version": "legacy"
        }
      }
    },
    "subscription": 70527
  }
}
```

Unsubscribe with [transactionUnsubscribe](transactionunsubscribe.md).

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
