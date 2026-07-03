---
description: Subscribe over WebSockets to a notification for each new block at the chosen commitment.
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# blockSubscribe

## Request

{% tabs %}
{% tab title="WebSocket message" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "blockSubscribe",
  "params": [
    "all",
    {
      "commitment": "finalized",
      "encoding": "base64",
      "transactionDetails": "full",
      "showRewards": true,
      "maxSupportedTransactionVersion": 0
    }
  ]
}
```
{% endtab %}
{% endtabs %}

Connect to `wss://<your-endpoint>.mainnet.rpcpool.com/<your-token>`.

## Parameters

**`filter`** · required

One of:

* `"all"` · every block.
* `{ "mentionsAccountOrProgram": <pubkey> }` · only blocks containing a transaction that mentions the given base-58 address.

**`config`** · optional

Solana's [`RpcBlockSubscribeConfig`](https://docs.rs/solana-client/latest/solana_client/rpc_config/struct.RpcBlockSubscribeConfig.html), the same config [transactionSubscribe](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/real-time-streaming/whirligig-websocket/transactionsubscribe) uses:

| Field                            | Default      | Description                                                              |
| -------------------------------- | ------------ | ----------------------------------------------------------------------- |
| `commitment`                     | `finalized`  | `confirmed` or `finalized`. `processed` is not supported.               |
| `encoding`                       | —            | Transaction encoding: `base58`, `base64`, `json`, or `jsonParsed`.      |
| `transactionDetails`             | `full`       | `full`, `accounts`, `signatures`, or `none`.                            |
| `showRewards`                    | `false`      | Include the block `rewards` array.                                      |
| `maxSupportedTransactionVersion` | —            | Highest transaction version to return; omit to receive only legacy transactions. |

## Response

```json
{
  "jsonrpc": "2.0",
  "result": 0,
  "id": 1
}
```

## Result

Subscription id. Pass this to [blockUnsubscribe](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/real-time-streaming/whirligig-websocket/blockunsubscribe).

## Notification

Updates arrive as `blockNotification` messages, each with a `context` (`slot`) and a `value`:

| Field   | Type            | Description                                                       |
| ------- | --------------- | ---------------------------------------------------------------- |
| `slot`  | u64             | Slot of the block.                                               |
| `block` | object \| null  | Block data, the same structure as [getBlock](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/historical-data/getblock). |
| `err`   | null \| object  | Error if the block failed to publish, otherwise `null`.          |

***

_Adapted from the_ [_Solana `blockSubscribe` reference_](https://solana.com/docs/rpc/websocket/blocksubscribe)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
