---
description: An enhanced Solana WebSocket API for browsers and existing WS clients, backed by a Dragon's Mouth gRPC stream.
---

# Whirligig WebSockets

Whirligig is a Rust proxy between your WebSocket client and a Dragon's Mouth gRPC stream. It speaks the standard Solana WebSocket API, so a browser or any existing WebSocket client subscribes over `wss://` instead of gRPC, with no change to your code or billing. Triton was the first Solana RPC provider to replace the native pubsub path with gRPC-backed WebSocket streaming.

Native Solana WebSockets run inside the RPC process, so notifications can lag or drop under load; they batch `processed` account updates to slot boundaries; they cap concurrent connections; and they have no full-transaction subscription. Whirligig serves the same API from dedicated streaming nodes, so at `processed` commitment account updates arrive intra-slot, up to 400 ms faster than native subscriptions.

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-bolt">:bolt:</i> <strong>transactionSubscribe</strong></td><td>A full-transaction subscription the native Solana WS API does not have, with account include, exclude, and required filters.</td><td></td></tr><tr><td><i class="fa-gauge-high">:gauge-high:</i> <strong>Intra-slot updates</strong></td><td>At processed commitment, account updates arrive up to 400 ms faster than native Solana WebSockets.</td><td></td></tr><tr><td><i class="fa-plug">:plug:</i> <strong>Drop-in Solana WS API</strong></td><td>Full parity with the standard Solana WebSocket API, so existing WS clients work unchanged.</td><td></td></tr><tr><td><i class="fa-radio">:radio:</i> <strong>Backed by Dragon's Mouth</strong></td><td>Translates a Dragon's Mouth gRPC stream into WebSocket messages, so a browser gets gRPC-grade data.</td><td></td></tr></tbody></table>

## Added capabilities

On top of the drop-in standard API, Whirligig adds:

* **`jsonParsed` encoding.** `accountSubscribe` and `programSubscribe` return structured objects for parsable programs (SPL Token, Token-2022, Stake, Vote, Nonce, Sysvar, Address Lookup Table, BPF Loader, Config) instead of raw byte arrays, with resolved mint metadata on token accounts.
* **Stable `blockSubscribe`.** Full block notifications with filters for commitment level, transaction detail, and reward visibility. The native method is flagged unstable.
* **No strict subscription cap.** Triton's bare-metal streaming nodes are stress-tested under hundreds of thousands of concurrent subscriptions.

## Methods

Whirligig has full parity with the [Solana WebSocket API](https://solana.com/docs/rpc/websocket): it serves the standard `solana-pubsub` endpoint, plus one added subscription, `transactionSubscribe`, that the native API does not have. Each subscribe method returns a subscription `id` and has a matching unsubscribe (see [Unsubscribe](#unsubscribe)). The methods with request and response examples below are the most common; the rest follow the standard Solana WebSocket spec.

| Method | Subscribe to | Notes |
| --- | --- | --- |
| `accountSubscribe` | An account's data, on change | Solana-compatible |
| `blockSubscribe` | New blocks | Solana-compatible |
| `logsSubscribe` | Transaction logs mentioning an address | Solana-compatible |
| `programSubscribe` | Accounts owned by a program, on change | Solana-compatible |
| `signatureSubscribe` | A signature's confirmation status | Solana-compatible |
| `slotSubscribe` | Each new slot | Solana-compatible |
| `rootSubscribe` | Each new root (the highest finalized slot) | Solana-compatible |
| `voteSubscribe` | Votes as they are observed in gossip | Solana-compatible (unstable Solana method) |
| `slotsUpdatesSubscribe` | Detailed slot-status updates | Solana-compatible (unstable Solana method) |
| `transactionSubscribe` | Full transactions matching a filter | Whirligig extension, not in the native Solana WS API |

## Connect

On Triton's shared infrastructure, your WebSocket connections route through Whirligig by default, with nothing to change, and run over HTTP/3 for lower-latency browser and mobile connections.

All standard Solana WebSocket subscriptions work as-is, with no library changes. The examples below subscribe to an account; the raw-WebSocket tab also shows `transactionSubscribe`, which is not available through `@solana/web3.js`.

{% tabs %}
{% tab title="JavaScript (web3.js)" %}
Point `@solana/web3.js` at the Whirligig `wsEndpoint`; everything else stays the same.

```javascript
import { Connection, PublicKey } from "@solana/web3.js";

const connection = new Connection(
  "https://<your-endpoint>.rpcpool.com/<your-token>",
  {
    wsEndpoint: "wss://<your-endpoint>.rpcpool.com/<your-token>",
    commitment: "processed",
  }
);

const subscriptionId = connection.onAccountChange(
  new PublicKey("<account-pubkey>"),
  (accountInfo, context) => {
    console.log("slot:", context.slot, "lamports:", accountInfo.lamports);
  },
  "processed"
);

// When done:
// await connection.removeAccountChangeListener(subscriptionId);
```
{% endtab %}

{% tab title="Rust" %}
Uses the standard [`solana-pubsub-client`](https://docs.rs/solana-pubsub-client) crate, the same client Agave ships with.

```rust
use futures::StreamExt;
use solana_account_decoder_client_types::UiAccountEncoding;
use solana_commitment_config::CommitmentConfig;
use solana_pubkey::Pubkey;
use solana_pubsub_client::nonblocking::pubsub_client::PubsubClient;
use solana_rpc_client_types::config::RpcAccountInfoConfig;
use std::str::FromStr;

#[tokio::main]
async fn main() {
    let client = PubsubClient::new("wss://<your-endpoint>.rpcpool.com/<your-token>")
        .await
        .expect("connect");

    let pubkey = Pubkey::from_str("<account-pubkey>").expect("valid pubkey");
    let config = RpcAccountInfoConfig {
        encoding: Some(UiAccountEncoding::JsonParsed),
        commitment: Some(CommitmentConfig::processed()),
        data_slice: None,
        min_context_slot: None,
    };

    let (mut stream, unsubscribe) = client
        .account_subscribe(&pubkey, Some(config))
        .await
        .expect("subscribe");

    while let Some(update) = stream.next().await {
        println!("{}", serde_json::to_string(&update).unwrap());
    }

    drop(stream);
    drop(unsubscribe);
    client.shutdown().await.expect("shutdown");
}
```
{% endtab %}

{% tab title="JavaScript (raw WebSocket)" %}
Required for `transactionSubscribe`, which returns full transaction data in a single call and is not available in `@solana/web3.js`.

```javascript
const ws = new WebSocket("wss://<your-endpoint>.rpcpool.com/<your-token>");

ws.addEventListener("open", () => {
  // Keep alive: Whirligig closes idle connections after 60 s
  setInterval(() => ws.send(JSON.stringify({ jsonrpc: "2.0", method: "ping" })), 30_000);

  ws.send(JSON.stringify({
    jsonrpc: "2.0", id: 1,
    method: "transactionSubscribe",
    params: [
      { vote: false, failed: false },
      { commitment: "confirmed", encoding: "jsonParsed", transactionDetails: "full" },
    ],
  }));
});

ws.addEventListener("message", ({ data }) => {
  const msg = JSON.parse(data);
  if (msg.method === "transactionNotification") {
    console.log("tx:", msg.params.result.signature);
  }
});
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Connections idle for over 60 seconds are closed, so keep them alive by sending `{"jsonrpc":"2.0","method":"ping"}`.
{% endhint %}

### Commitment levels

Whirligig supports all three commitment levels, set per subscription in the `config` argument, the same way as Dragon's Mouth gRPC:

* **`processed`**: the highest slot of the heaviest fork the node has processed. Not yet confirmed or finalized; if forks are present, it is the fork most likely to finalize.
* **`confirmed`**: the highest slot voted on by a supermajority of the cluster.
* **`finalized`**: the highest slot that has reached maximum lockout, recognized as finalized by a supermajority.

## Subscribe

Each subscribe request returns a subscription `id` you later pass to its unsubscribe method.

### accountSubscribe

{% tabs %}
{% tab title="Request" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "accountSubscribe",
  "params": [
    "SysvarC1ock11111111111111111111111111111111",
    {
      "encoding": "base58",
      "commitment": "finalized"
    }
  ]
}

```
{% endtab %}

{% tab title="Response" %}
```json
{
  "jsonrpc": "2.0",
  "method": "accountNotification",
  "params": {
    "result": {
      "context": {
        "apiVersion": "1.16.1",
        "slot": 206122916
      },
      "value": {
        "data": [
          "9AAG2zomYcUVKQfondw13yz193crQ5R5zmzdEojhMPXiyYEjD8viLrb",
          "base58"
        ],
        "executable": false,
        "lamports": 1169280,
        "owner": "Sysvar1111111111111111111111111111111111111",
        "rentEpoch": 361,
        "space": 40
      }
    },
    "subscription": 0
  }
}
```
{% endtab %}
{% endtabs %}

### blockSubscribe

{% tabs %}
{% tab title="Request" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "blockSubscribe",
  "params": [
    "all",
    {
      "commitment": "finalized",
      "maxSupportedTransactionVersion": 1,
      "showRewards": false,
      "transactionDetails": "none"
    }
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "jsonrpc": "2.0",
  "method": "blockNotification",
  "params": {
    "result": {
      "context": {
        "apiVersion": "1.16.1",
        "slot": 206125999
      },
      "value": {
        "block": {
          "blockHeight": 188652385,
          "blockTime": 1689709529,
          "blockhash": "74iqy9xdFvenLkFizkoBkkLRWTyygEeCiiN3B8Z8ma2D",
          "parentSlot": 206125998,
          "previousBlockhash": "Cq5nfgu9Y5KF47J9qj5CTYmt8D5CxBpw8HCRqQZ7o7i9"
        },
        "err": null,
        "slot": 206125999
      }
    },
    "subscription": 0
  }
}
```
{% endtab %}
{% endtabs %}

### logsSubscribe

{% tabs %}
{% tab title="Request" %}
```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "logsSubscribe",
  "params": [
    {
      "mentions": ["8BnEgHoWFysVcuFFX7QztDmzuH8r5ZFvyP3sYwn1XTh6"]
    },
    {
      "commitment": "finalized"
    }
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "jsonrpc": "2.0",
  "method": "logsNotification",
  "params": {
    "result": {
      "context": {
        "apiVersion": "1.16.1",
        "slot": 206126709
      },
      "value": {
        "err": null,
        "logs": [
          "Program FC81tbGt6JWRXidaWYFXxGnTk4VgobhJHATvTRVMqgWj invoke [1]",
          "Program log: process_instruction: FC81tbGt6JWRXidaWYFXxGnTk4VgobhJHATvTRVMqgWj: 3 accounts, data=[12]",
          "Program log: Instruction: UpdateLendingPool",
          "Program log: current_borrow_rate: 73081853393945351",
          "Program FC81tbGt6JWRXidaWYFXxGnTk4VgobhJHATvTRVMqgWj consumed 23800 of 1000000 compute units",
          "Program FC81tbGt6JWRXidaWYFXxGnTk4VgobhJHATvTRVMqgWj success",
          "Program FC81tbGt6JWRXidaWYFXxGnTk4VgobhJHATvTRVMqgWj invoke [1]",
          "Program log: process_instruction: FC81tbGt6JWRXidaWYFXxGnTk4VgobhJHATvTRVMqgWj: 3 accounts, data=[12]",
          "Program log: Instruction: UpdateLendingPool",
          "Program log: current_borrow_rate: 82164236984041212",
          "Program FC81tbGt6JWRXidaWYFXxGnTk4VgobhJHATvTRVMqgWj consumed 28159 of 976200 compute units",
          "Program FC81tbGt6JWRXidaWYFXxGnTk4VgobhJHATvTRVMqgWj success",
          "Program 2nAAsYdXF3eTQzaeUQS3fr4o782dDg8L28mX39Wr5j8N invoke [1]",
          "Program log: Instruction: LiquidateUnstakeLp",
          "Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA invoke [2]",
          "Program log: Instruction: Transfer",
          "Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA consumed 4740 of 879873 compute units",
          "Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA success",
          "Program 2nAAsYdXF3eTQzaeUQS3fr4o782dDg8L28mX39Wr5j8N consumed 79765 of 948041 compute units",
          "Program 2nAAsYdXF3eTQzaeUQS3fr4o782dDg8L28mX39Wr5j8N success",
          "Program 2nAAsYdXF3eTQzaeUQS3fr4o782dDg8L28mX39Wr5j8N invoke [1]",
          "Program log: Instruction: RemoveLiquidity",
          "Program 2nAAsYdXF3eTQzaeUQS3fr4o782dDg8L28mX39Wr5j8N consumed 24767 of 868276 compute units",
          "Program 2nAAsYdXF3eTQzaeUQS3fr4o782dDg8L28mX39Wr5j8N success",
          "Program 2nAAsYdXF3eTQzaeUQS3fr4o782dDg8L28mX39Wr5j8N invoke [1]",
          "Program log: Instruction: SwapAndWithdraw",
          "Program log: withdraw_type: 2,\\n            user_info.pending_repay_0: 0,\\n            user_info.pending_repay_1: 0,\\n            user_info.tkn_0: 0,\\n            user_info.tkn_1: 0,\\n            ",
          "Program log: swap_direction: 0,\\n            amount_in: 0\\n            ",
          "Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA invoke [2]",
          "Program log: Instruction: Transfer",
          "Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA consumed 4785 of 815778 compute units",
          "Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA success",
          "Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA invoke [2]",
          "Program log: Instruction: Transfer",
          "Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA consumed 4831 of 807689 compute units",
          "Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA success",
          "Program 2nAAsYdXF3eTQzaeUQS3fr4o782dDg8L28mX39Wr5j8N consumed 47497 of 843509 compute units",
          "Program 2nAAsYdXF3eTQzaeUQS3fr4o782dDg8L28mX39Wr5j8N success"
        ],
        "signature": "5UPB2AEWgUafVDEw2xTkup8CUedxCMNL4k7EMuxtcyi1WnYLmpXeYUfBANpnPnpSz6LRwwLnBQf7tViKSFT7ojiB"
      }
    },
    "subscription": 0
  }
}
```
{% endtab %}
{% endtabs %}

### programSubscribe

{% tabs %}
{% tab title="Request" %}
```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "programSubscribe",
  "params": [
    "srmqPvymJeFKQ4zGQed1GFppgkRHL9kaELCbyksJtPX",
    {
      "encoding": "base64+zstd",
      "commitment": "finalized"
    }
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "jsonrpc": "2.0",
  "method": "programNotification",
  "params": {
    "result": {
      "context": {
        "apiVersion": "1.16.1",
        "slot": 206127070
      },
      "value": {
        "account": {
          "data": [
            "KLUv/QBYDQEAkHNlcnVtQQABAAQAcGFkZGluZwUA03+gI0CxQgAYeABG",
            "base64+zstd"
          ],
          "executable": false,
          "lamports": 457104960,
          "owner": "srmqPvymJeFKQ4zGQed1GFppgkRHL9kaELCbyksJtPX",
          "rentEpoch": 0,
          "space": 65548
        },
        "pubkey": "5qRKrGPYaQmazdtMnia1iC3gFbpgW4CT7KPrHq9nJGm5"
      }
    },
    "subscription": 0
  }
}
```
{% endtab %}
{% endtabs %}

### signatureSubscribe

{% tabs %}
{% tab title="Request" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "signatureSubscribe",
  "params": [
    "2EBVM6cB8vAAD93Ktr6Vd8p67XPbQzCJX47MpReuiCXJAtcjaxpvWpcg9Ege1Nr5Tk3a2GFrByT7WPBjdsTycY9b",
    {
      "commitment": "finalized"
    }
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "jsonrpc": "2.0",
  "method": "signatureNotification",
  "params": {
    "result": {
      "context": {
        "apiVersion": "1.16.1",
        "slot": 206127070
      },
      "value": {
        "err": null
      }
    },
    "subscription": 24006
  }
}
```
{% endtab %}
{% endtabs %}

### slotSubscribe

{% tabs %}
{% tab title="Request" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "slotSubscribe"
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "jsonrpc": "2.0",
  "method": "slotNotification",
  "params": {
    "result": {
      "parent": 206128153,
      "root": 206128115,
      "slot": 206128154
    },
    "subscription": 0
  }
}
```
{% endtab %}
{% endtabs %}

### transactionSubscribe

Not in the native Solana WebSocket API. One subscription returns the full transaction, including metadata, log messages, token balances, and compute units, replacing the native `signatureSubscribe` + `getTransaction` loop and its two billed calls per transaction. Takes a `filter` (all fields optional) and a `config` ([`RpcBlockSubscribeConfig`](https://docs.rs/solana-client/latest/solana_client/rpc_config/struct.RpcBlockSubscribeConfig.html)).

{% tabs %}
{% tab title="Request" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "transactionSubscribe",
  "params": [
    {
      "vote": false,
      "failed": false,
      "signature": "2EBVM6cB8vAAD93Ktr6Vd8p67XPbQzCJX47MpReuiCXJAtcjaxpvWpcg9Ege1Nr5Tk3a2GFrByT7WPBjdsTycY9b",
      "accounts": {
        "include": ["5qRKrGPYaQmazdtMnia1iC3gFbpgW4CT7KPrHq9nJGm5"],
        "exclude": ["srmqPvymJeFKQ4zGQed1GFppgkRHL9kaELCbyksJtPX"],
        "required": ["TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"]
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

{% tab title="Response" %}
```json
{
  "jsonrpc": "2.0",
  "method": "transactionNotification",
  "params": {
    "result": {
      "context": {
        "apiVersion": "1.16.1",
        "slot": 206132915
      },
      "value": {
        "slot": 241068727,
        "signature": "2EBVM6cB8vAAD93Ktr6Vd8p67XPbQzCJX47MpReuiCXJAtcjaxpvWpcg9Ege1Nr5Tk3a2GFrByT7WPBjdsTycY9b",
        "transaction": {
          "meta": {
            "computeUnitsConsumed": 27426,
            "err": null,
            "fee": 5000,
            "innerInstructions": [
              {
                "index": 3,
                "instructions": [
                  {
                    "accounts": [
                      7,
                      3,
                      11
                    ],
                    "data": "3DTZbgwsozUF",
                    "programIdIndex": 15,
                    "stackHeight": null
                  },
                  {
                    "accounts": [
                      9,
                      8,
                      11
                    ],
                    "data": "3DTZbgwsozUF",
                    "programIdIndex": 15,
                    "stackHeight": null
                  }
                ]
              }
            ],
            "loadedAddresses": {
              "readonly": [],
              "writable": []
            },
            "logMessages": [
              "Program ComputeBudget111111111111111111111111111111 invoke [1]",
              "Program ComputeBudget111111111111111111111111111111 success",
              "Program GDDMwNyyx8uB6zrqwBFHjLLG3TBYk2F8Az4yrQC5RzMp invoke [1]",
              "Program log: Sequence in order | sequence_num=755609 | last_known=755608",
              "Program GDDMwNyyx8uB6zrqwBFHjLLG3TBYk2F8Az4yrQC5RzMp consumed 3398 of 300000 compute units",
              "Program GDDMwNyyx8uB6zrqwBFHjLLG3TBYk2F8Az4yrQC5RzMp success",
              "Program srmqPvymJeFKQ4zGQed1GFppgkRHL9kaELCbyksJtPX invoke [1]",
              "Program srmqPvymJeFKQ4zGQed1GFppgkRHL9kaELCbyksJtPX consumed 3820 of 296602 compute units",
              "Program srmqPvymJeFKQ4zGQed1GFppgkRHL9kaELCbyksJtPX success",
              "Program srmqPvymJeFKQ4zGQed1GFppgkRHL9kaELCbyksJtPX invoke [1]",
              "Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA invoke [2]",
              "Program log: Instruction: Transfer",
              "Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA consumed 4740 of 285091 compute units",
              "Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA success",
              "Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA invoke [2]",
              "Program log: Instruction: Transfer",
              "Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA consumed 4740 of 277786 compute units",
              "Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA success",
              "Program srmqPvymJeFKQ4zGQed1GFppgkRHL9kaELCbyksJtPX consumed 20208 of 292782 compute units",
              "Program srmqPvymJeFKQ4zGQed1GFppgkRHL9kaELCbyksJtPX success"
            ],
            "postBalances": [
              10194778999,
              1224960,
              457104960,
              2039280,
              23357760,
              1825496640,
              3591360,
              2039280,
              2039280,
              2039280,
              457104960,
              0,
              1,
              1141440,
              1141440,
              934087680
            ],
            "postTokenBalances": [
              {
                "accountIndex": 3,
                "mint": "DezXAZ8z7PnrnRJjz3wXBoRgixCa6xjnB7YaB1pPB263",
                "owner": "ASx1wk74GLZsxVrYiBkNKiViPLjnJQVGxKrudRgPir4A",
                "programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
                "uiTokenAmount": {
                  "amount": "4973704995974757",
                  "decimals": 5,
                  "uiAmount": 49737049959.74757,
                  "uiAmountString": "49737049959.74757"
                }
              },
              {
                "accountIndex": 7,
                "mint": "DezXAZ8z7PnrnRJjz3wXBoRgixCa6xjnB7YaB1pPB263",
                "owner": "3oQLKk1TyyXMT14p2i8p95v5jKqsQ5qzZGwxZCTnFC7p",
                "programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
                "uiTokenAmount": {
                  "amount": "3167826900000000",
                  "decimals": 5,
                  "uiAmount": 31678269000,
                  "uiAmountString": "31678269000"
                }
              },
              {
                "accountIndex": 8,
                "mint": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
                "owner": "ASx1wk74GLZsxVrYiBkNKiViPLjnJQVGxKrudRgPir4A",
                "programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
                "uiTokenAmount": {
                  "amount": "148629334218",
                  "decimals": 6,
                  "uiAmount": 148629.334218,
                  "uiAmountString": "148629.334218"
                }
              },
              {
                "accountIndex": 9,
                "mint": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
                "owner": "3oQLKk1TyyXMT14p2i8p95v5jKqsQ5qzZGwxZCTnFC7p",
                "programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
                "uiTokenAmount": {
                  "amount": "5797710875",
                  "decimals": 6,
                  "uiAmount": 5797.710875,
                  "uiAmountString": "5797.710875"
                }
              }
            ],
            "preBalances": [
              10194783999,
              1224960,
              457104960,
              2039280,
              23357760,
              1825496640,
              3591360,
              2039280,
              2039280,
              2039280,
              457104960,
              0,
              1,
              1141440,
              1141440,
              934087680
            ],
            "preTokenBalances": [
              {
                "accountIndex": 3,
                "mint": "DezXAZ8z7PnrnRJjz3wXBoRgixCa6xjnB7YaB1pPB263",
                "owner": "ASx1wk74GLZsxVrYiBkNKiViPLjnJQVGxKrudRgPir4A",
                "programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
                "uiTokenAmount": {
                  "amount": "4973704995974757",
                  "decimals": 5,
                  "uiAmount": 49737049959.74757,
                  "uiAmountString": "49737049959.74757"
                }
              },
              {
                "accountIndex": 7,
                "mint": "DezXAZ8z7PnrnRJjz3wXBoRgixCa6xjnB7YaB1pPB263",
                "owner": "3oQLKk1TyyXMT14p2i8p95v5jKqsQ5qzZGwxZCTnFC7p",
                "programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
                "uiTokenAmount": {
                  "amount": "3167826900000000",
                  "decimals": 5,
                  "uiAmount": 31678269000,
                  "uiAmountString": "31678269000"
                }
              },
              {
                "accountIndex": 8,
                "mint": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
                "owner": "ASx1wk74GLZsxVrYiBkNKiViPLjnJQVGxKrudRgPir4A",
                "programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
                "uiTokenAmount": {
                  "amount": "148629334218",
                  "decimals": 6,
                  "uiAmount": 148629.334218,
                  "uiAmountString": "148629.334218"
                }
              },
              {
                "accountIndex": 9,
                "mint": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
                "owner": "3oQLKk1TyyXMT14p2i8p95v5jKqsQ5qzZGwxZCTnFC7p",
                "programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
                "uiTokenAmount": {
                  "amount": "5797710875",
                  "decimals": 6,
                  "uiAmount": 5797.710875,
                  "uiAmountString": "5797.710875"
                }
              }
            ],
            "rewards": null,
            "status": {
              "Ok": null
            }
          },
          "transaction": [
            "nMXJYDy61Acdo2FY39i2ENUwNCMDYBWgRi6dNBgJ...YhdoeiKMqZWo",
            "base58"
          ]
        },
        "error": null
      }
    },
    "subscription": 0
  }
}
```
{% endtab %}
{% endtabs %}

## Unsubscribe

Every subscribe request returns the subscription `id` on success:

```json
{ "jsonrpc": "2.0", "result": 42, "id": 1 }
```

Pass that `id` to the matching unsubscribe method (`accountUnsubscribe`, `slotUnsubscribe`, and so on):

```json
{ "id": 1, "jsonrpc": "2.0", "method": "slotUnsubscribe", "params": [42] }
```

The result is `true` if the subscription was active and removed, `false` if it did not exist:

```json
{ "jsonrpc": "2.0", "result": true, "id": 1 }
```

## Client

A reference CLI client is on GitHub: [yellowstone-whirligig-client](https://github.com/rpcpool/yellowstone-whirligig-client).

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-radio">:radio:</i> <strong>Dragon's Mouth gRPC</strong></td><td>Sub-slot real-time updates for accounts, transactions, slots, and blocks via gRPC.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/dragon-s-mouth-grpc">Dragon's Mouth gRPC</a></td></tr><tr><td><i class="fa-list-check">:list-check:</i> <strong>Streaming best practices</strong></td><td>Filtering, reconnect, and commitment guidance across the streaming services.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/best-practices">Streaming best practices</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
