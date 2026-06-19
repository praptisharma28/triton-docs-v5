---
description: >-
  Connect to Dragon's Mouth, subscribe to a live stream, and handle accounts,
  transactions, slots, and blocks over Yellowstone gRPC.
---

# Start streaming with Yellowstone gRPC

Dragon's Mouth is Triton's Geyser-fed gRPC interface for streaming Solana data: account writes, transactions, slots, blocks, block metadata, and entries. You subscribe once and the node pushes strongly typed Protobuf updates the moment they arrive, bypassing the JSON-RPC HTTP layer. Use it for backend workloads that need the lowest possible latency and fresh, structured data: trading, indexing, analytics, and DeFi. gRPC is unsupported by web browsers, so Dragon's Mouth targets backend software; for a browser-facing WebSocket interface, use Whirligig.

## Prerequisites

* A Dragon's Mouth endpoint and an `x-token`.
* A gRPC client. Official sample clients exist for Rust, Go, and NodeJS/TypeScript. You can also test with the generic `grpcurl` client.

For NodeJS/TypeScript, install the client:

```bash
npm install --save @triton-one/yellowstone-grpc

# or, for yarn:

yarn add @triton-one/yellowstone-grpc
```

{% hint style="info" %}
The underlying gRPC proto can change, so test with a client matching the current version of the interface. Protobuf files live in the [yellowstone-grpc repo](https://github.com/rpcpool/yellowstone-grpc/tree/master/yellowstone-grpc-proto/proto) or the [`yellowstone-grpc-proto`](https://crates.io/crates/yellowstone-grpc-proto) crate.
{% endhint %}

## Minimal subscriber

Connect, subscribe to one program's account updates plus slots, and log each update.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import Client, {
  CommitmentLevel,
  SubscribeRequest,
} from "@triton-one/yellowstone-grpc";

async function main() {
  // Connect to the gRPC endpoint
  const client = new Client("https://your-endpoint.rpcpool.com", "your-x-token", {
    grpcMaxDecodingMessageSize: 64 * 1024 * 1024, // 64MiB
  });

  await client.connect();

  // Subscribe to events
  const stream = await client.subscribe();

  // Handle stream events
  const streamClosed = new Promise<void>((resolve, reject) => {
    stream.on("error", (error) => {
      reject(error);
      stream.end();
    });
    stream.on("end", resolve);
    stream.on("close", resolve);
  });

  // Process incoming data
  stream.on("data", (data) => {
    if (data.slot) {
      console.log(`Slot: ${data.slot.slot}, Status: ${data.slot.status}`);
    }
    if (data.account) {
      console.log(`Account update at slot: ${data.account.slot}`);
    }
  });

  // Build and send subscription request
  const request: SubscribeRequest = {
    accounts: {
      client: {
        account: [],
        owner: ["675kPX9MHTjS2zt1qfr1NYHuzeLXfQM9H24wFSUt1Mp8"], // Raydium AMM
        filters: [],
      },
    },
    slots: {
      client: { filterByCommitment: true },
    },
    transactions: {},
    transactionsStatus: {},
    entry: {},
    blocks: {},
    blocksMeta: {},
    commitment: CommitmentLevel.PROCESSED,
    accountsDataSlice: [],
    ping: undefined,
  };

  await new Promise<void>((resolve, reject) => {
    stream.write(request, (err) => (err ? reject(err) : resolve()));
  });

  await streamClosed;
}

main().catch(console.error);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use {
    futures::StreamExt,
    std::collections::HashMap,
    yellowstone_grpc_client::GeyserGrpcClient,
    yellowstone_grpc_proto::prelude::{
        CommitmentLevel, SubscribeRequest, SubscribeRequestFilterSlots,
        SubscribeRequestFilterAccounts, subscribe_update::UpdateOneof,
    },
};

#[tokio::main]
async fn main() -> anyhow::Result<()> {
    // Connect to the gRPC endpoint
    let mut client = GeyserGrpcClient::build_from_shared("https://your-endpoint.rpcpool.com")?
        .x_token(Some("your-x-token".to_string()))?
        .connect()
        .await?;

    // Build subscription request
    let mut slots = HashMap::new();
    slots.insert(
        "client".to_string(),
        SubscribeRequestFilterSlots {
            filter_by_commitment: Some(true),
            interslot_updates: Some(false),
        },
    );

    let mut accounts = HashMap::new();
    accounts.insert(
        "client".to_string(),
        SubscribeRequestFilterAccounts {
            account: vec![], // specific account pubkeys
            owner: vec!["675kPX9MHTjS2zt1qfr1NYHuzeLXfQM9H24wFSUt1Mp8".to_string()], // Raydium AMM
            filters: vec![],
            nonempty_txn_signature: None,
        },
    );

    let request = SubscribeRequest {
        slots,
        accounts,
        commitment: Some(CommitmentLevel::Processed as i32),
        ..Default::default()
    };

    // Subscribe and handle updates
    let (mut _tx, mut stream) = client.subscribe_with_request(Some(request)).await?;

    while let Some(message) = stream.next().await {
        match message?.update_oneof {
            Some(UpdateOneof::Slot(slot)) => {
                println!("Slot: {}, Status: {}", slot.slot, slot.status);
            }
            Some(UpdateOneof::Account(account)) => {
                println!("Account update at slot: {}", account.slot);
            }
            _ => {}
        }
    }

    Ok(())
}
```
{% endtab %}
{% endtabs %}

The subscribe stream is bi-directional: you receive updates on `data` and send requests by writing to the same stream. The NodeJS client is asynchronous, so run all calls inside an async function.

## Subscribe request shape

Every subscription is a single JSON request object. Each stream type is a map of client-assigned labels to filters; an empty map (`{}`) means that stream is off. Labels (for example `wsol/usdc`) tag the updates you receive, so you can mix accounts, programs, blocks, and slots in one request.

| Stream         | Request key                          | Empty value |
| -------------- | ------------------------------------ | ----------- |
| Account writes | `accounts`                           | `{}`        |
| Transactions   | `transactions`                       | `{}`        |
| Slots          | `slots`                              | `{}`        |
| Blocks         | `blocks`                             | `{}`        |
| Block metadata | `blocks_meta` (NodeJS: `blocksMeta`) | `{}`        |

The examples below show the raw gRPC JSON alongside the NodeJS form. Field names differ between them: gRPC uses snake\_case (`account_include`, `blocks_meta`, `accounts_data_slice`), NodeJS uses camelCase (`accountInclude`, `blocksMeta`, `accountsDataSlice`).

## Subscribe to accounts

Subscribe to a single account by pubkey on `confirmed` commitment. Here `wsol/usdc` is a client-assigned label.

{% tabs %}
{% tab title="gRPC" %}
```json
{"slots": { "slots": {} }, "accounts": { "wsol/usdc": { "account": ["8BnEgHoWFysVcuFFX7QztDmzuH8r5ZFvyP3sYwn1XTh6"] } }, "transactions": {}, "blocks": {}, "blocks_meta": {}, "accounts_data_slice": [], "commitment": 1}
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
import { CommitmentLevel } from "@triton-one/yellowstone-grpc";

const request = {
  "slots": {
    "slots": {}
  },
  "accounts": {
    "wsol/usdc": {
      "account": ["8BnEgHoWFysVcuFFX7QztDmzuH8r5ZFvyP3sYwn1XTh6"]
    }
  },
  "transactions": {},
  "blocks": {},
  "blocksMeta": {},
  "accountsDataSlice": [],
  "commitment": CommitmentLevel.CONFIRMED
};
```
{% endtab %}
{% endtabs %}

To subscribe to all accounts owned by a program, pass `owner`. This example streams Solend account writes on `processed`.

{% tabs %}
{% tab title="gRPC" %}
```json
{"slots": { "slots": {} }, "accounts": { "solend": {  "owner": ["So1endDq2YkqhipRh3WViPa8hdiSpxWy6z3Z6tMCpAo"] } }, "transactions": {}, "blocks": {}, "blocks_meta": {}, "accounts_data_slice": [], "commitment": 0}
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
import { CommitmentLevel } from "@triton-one/yellowstone-grpc";

const request = {
  "slots": {
    "slots": {}
  },
  "accounts": {
    "solend": {
      "owner": ["So1endDq2YkqhipRh3WViPa8hdiSpxWy6z3Z6tMCpAo"]
    }
  },
  "transactions": {},
  "blocks": {},
  "blocksMeta": {},
  "accountsDataSlice": [],
  "commitment": CommitmentLevel.PROCESSED
}
```
{% endtab %}
{% endtabs %}

Add `filters` and `accounts_data_slice` to narrow the match and trim the payload. This subscribes to USDC Tokenkeg accounts and returns only 40 bytes of account data instead of all 165 (`offset` 32 gives `owner` and `lamports`).

{% tabs %}
{% tab title="gRPC" %}
```json
{
    "accounts": {
        "usdc": {
            "owner": ["TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"],
            "filters": [{
                "token_account_state": true
            }, {
                "memcmp": {
                    "offset": 0,
                    "data": {
                        "base58": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v"
                    }
                }
            }]
        }
    },
    "accounts_data_slice": [{ "offset": 32, "length": 40 }]
}
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
import { CommitmentLevel } from "@triton-one/yellowstone-grpc";

const request = {
  "slots": {},
  "accounts": {
    "usdc": {
      "owner": ["TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"],
      "filters": [{
          "tokenAccountState": true
      }, {
          "memcmp": {
              "offset": 0,
              "data": {
                  "base58": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v"
              }
          }
      }]
    }
  },
  "transactions": {},
  "blocks": {},
  "blocksMeta": {},
  "entry": {},
  "commitment": CommitmentLevel.CONFIRMED,
  "accountsDataSlice": [{ "offset": 32, "length": 40 }],
};
```
{% endtab %}
{% endtabs %}

## Subscribe to transactions

If every transaction field is empty, all transactions are broadcast. Otherwise fields work as logical `AND`, and values in arrays as logical `OR`. You can include or exclude vote transactions and failed transactions.

This streams all finalized non-vote, non-failed transactions:

{% tabs %}
{% tab title="gRPC" %}
```json
{"slots": { "slots": {} }, "accounts": {}, "transactions": { "alltxs": { "vote": false, "failed": false }}, "blocks": {}, "blocks_meta": {}, "accounts_data_slice": [], "commitment": 2}
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
import { CommitmentLevel } from "@triton-one/yellowstone-grpc";

const request = {
  "slots": {
    "slots": {}
  },
  "accounts": {},
  "transactions": {
    "alltxs": {
      "vote": false,
      "failed": false
    }
  },
  "blocks": {},
  "blocksMeta": {},
  "accountsDataSlice": [],
  "commitment": CommitmentLevel.FINALIZED
};
```
{% endtab %}
{% endtabs %}

Filter to transactions mentioning specific accounts with `account_include`, exclude with `account_exclude`, and combine the two. This streams non-vote transactions that mention one account but not another:

{% tabs %}
{% tab title="gRPC" %}
```json
{"slots": { "slots": {} }, "accounts": {}, "transactions": { "serum": { "account_include": [ "9xQeWvG816bUx9EPjHmaT23yvVM2ZWbrrpZb9PusVFin" ], "account_exclude": [ "9wFFyRfZBsuAha4YcuxcXLKwMxJR43S7fPfQLusDBzvT" ] }}, "blocks": {}, "blocks_meta": {}, "accounts_data_slice": []}
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
const request = {
  "slots": {
    "slots": {}
  },
  "accounts": {},
  "transactions": {
    "serum": {
      "accountInclude": [
        "9xQeWvG816bUx9EPjHmaT23yvVM2ZWbrrpZb9PusVFin"
      ],
      "accountExclude": [
        "9wFFyRfZBsuAha4YcuxcXLKwMxJR43S7fPfQLusDBzvT"
      ]
    }
  },
  "blocks": {},
  "blocksMeta": {},
  "accountsDataSlice": []
};
```
{% endtab %}
{% endtabs %}

To watch a single transaction as it confirms and finalizes, subscribe by signature:

{% tabs %}
{% tab title="gRPC" %}
```json
{"slots": {}, "accounts": {}, "transactions": { "sign": { "signature": "5rp2hL9b6kexex11Mugfs3vfU9GhieKruj4CkFFSnu52WLxiGn4VcLLwsB62XURhMmT1j4CZiXT6FFtYbXsLq2Zs"}}, "blocks": {}, "blocks_meta": {}, "accounts_data_slice": []}
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
const request = {
  "slots": {},
  "accounts": {},
  "transactions": {
    "sign": {
      "signature": "5rp2hL9b6kexex11Mugfs3vfU9GhieKruj4CkFFSnu52WLxiGn4VcLLwsB62XURhMmT1j4CZiXT6FFtYbXsLq2Zs"
    }
  },
  "blocks": {},
  "blocksMeta": {},
  "accountsDataSlice": []
};
```
{% endtab %}
{% endtabs %}

## Subscribe to slots

Slot notifications need only a label.

{% tabs %}
{% tab title="gRPC" %}
```json
{"slots": { "incoming_slots": {} }, "accounts": {}, "transactions": {}, "blocks": {}, "blocks_meta": {}, "accounts_data_slice": []}
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
const request = {
  "slots": {
    "incoming_slots": {}
  },
  "accounts": {},
  "transactions": {},
  "blocks": {},
  "blocksMeta": {},
  "accountsDataSlice": []
};
```
{% endtab %}
{% endtabs %}

## Subscribe to blocks and block metadata

`blocks` returns each block as it is produced, including its transactions by default. Toggle `include_transactions` and `include_accounts`, or filter with `account_include` to receive only blocks where a given account is mentioned.

{% tabs %}
{% tab title="gRPC" %}
```json
{"slots": {}, "accounts": { }, "transactions": {}, "blocks": { "blocks": {"include_transactions": false, "include_accounts": true} }, "blocks_meta": {}, "accounts_data_slice": []}
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
const request = {
  "slots": {},
  "accounts": {},
  "transactions": {},
  "blocks": {
    "blocks": {
      "includeTransactions": false,
      "includeAccounts": true
    }
  },
  "blocksMeta": {},
  "accountsDataSlice": []
};
```
{% endtab %}
{% endtabs %}

For block-completion notifications without the transactions, use `blocks_meta`:

{% tabs %}
{% tab title="gRPC" %}
```json
{"slots": {}, "accounts": {}, "transactions": {}, "blocks": {}, "blocks_meta": { "blockmetadata": {} }, "accounts_data_slice": []}
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
const request = {
  "slots": {},
  "accounts": {},
  "transactions": {},
  "blocks": {},
  "blocksMeta": {
    "blockmetadata": {}
  },
  "accountsDataSlice": []
};
```
{% endtab %}
{% endtabs %}

## Modifying and unsubscribing

The Subscribe method is a bi-directional stream, so you can change your subscription by writing a new request. The new request entirely overwrites the previous one, so keep a local register of the full subscription config you want. To unsubscribe from everything while keeping the connection open, send empty maps for all streams:

{% tabs %}
{% tab title="gRPC" %}
```json
{"slots": {}, "accounts": {}, "transactions": {}, "blocks": {}, "blocks_meta": {}}
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
const request = {
  "slots": {},
  "accounts": {},
  "transactions": {},
  "blocks": {},
  "blocksMeta": {},
  "accountsDataSlice": []
};
```
{% endtab %}
{% endtabs %}

## Managing commitment

Streams run on `processed` commitment by default. You can also request `confirmed` or `finalized`, in which case Dragon's Mouth buffers updates and releases them once they reach that level. For maximum performance, handle commitment client-side instead.

```protobuf
enum CommitmentLevel {
  PROCESSED = 0;
  CONFIRMED = 1;
  FINALIZED = 2;
}
```

Working at `processed` lets you process transactions as soon as they arrive and commit to them only once you know they are confirmed or finalized, so you can surface UI changes faster.

To manage `confirmed` and `finalized` client-side, buffer events by the slot attached to each transaction or account write, then subscribe to slot notifications. Release a slot's buffer when you receive its notification at the commitment level you want. All transaction and account-write notifications for a slot arrive before that slot's confirmed or finalized notification.

{% hint style="warning" %}
Due to a quirk in how Geyser works on Solana (fixed in `master`), not every finalized slot notification is issued. Whenever you see a `finalized` slot notification, retroactively mark its ancestors as `finalized` too, even without a notification for them.
{% endhint %}

## Keep-alive pings

Some cloud providers (for example Cloudflare) close idle streams. Send periodic pings to keep the connection open; the server responds with a `pong` every 15 seconds.

```typescript
const PING_INTERVAL_MILLISECONDS = 30000;

// Ping request
const pingRequest: SubscribeRequest = {
    ping: { id: 1 },
    accounts: {},
    accountsDataSlice: [],
    transactions: {},
    transactionsStatus: {},
    blocks: {},
    blocksMeta: {},
    entry: {},
    slots: {},
};

// Sending pings periodically
setInterval(async () => {
  await new Promise<void>((resolve, reject) => {
    stream.write(pingRequest, (err) => {
      if (err === null || err === undefined) {
        resolve();
      } else {
        reject(err);
      }
    });
  }).catch((reason) => {
    console.error(reason);
    throw reason;
  });
}, PING_INTERVAL_MILLISECONDS);

// Handling pong responses
// This goes in your `data` handler
stream.on("data", (data) => {
if (data.pong) {
    console.log("Received Pong response");
  }
});
```

## Reconnect and replay from a slot

Dragon's Mouth can replay recently buffered updates by setting `from_slot` on the `SubscribeRequest`, mainly to recover from short disconnections. The server first replays buffered updates from that slot, then continues streaming live on the same connection using the same filters and commitment level.

Replay logic:

1. Track the latest slot your application has processed.
2. On disconnect, reconnect and resubscribe with `from_slot` set to that slot.
3. If the requested slot is no longer available, fall back to a fresh live subscription or your own backfill path.

Replay covers only the server's retained replay window. Replay starts at a slot boundary, so reconnecting from your last processed slot may yield duplicate updates from that slot: deduplicate client-side. To find the earliest replayable slot, call the unary `SubscribeReplayInfo` RPC and read `first_available`; if `from_slot` is older, the request fails and you should retry with a newer slot.

{% tabs %}
{% tab title="gRPC" %}
```json
{
  "slots": {
    "incoming_slots": {}
  },
  "accounts": {},
  "transactions": {
    "alltxs": {
      "vote": false,
      "failed": false
    }
  },
  "blocks": {},
  "blocks_meta": {},
  "accounts_data_slice": [],
  "from_slot": 382001234
}
```
{% endtab %}

{% tab title="NodeJS" %}
```typescript
const request = {
  slots: {
    incoming_slots: {}
  },
  accounts: {},
  transactions: {
    alltxs: {
      vote: false,
      failed: false
    }
  },
  blocks: {},
  blocksMeta: {},
  accountsDataSlice: [],
  fromSlot: 382001234
};
```
{% endtab %}
{% endtabs %}

Check replay availability before reconnecting:

```typescript
const info = await client.subscribeReplayInfo();
console.log(info.firstAvailable);
```

{% hint style="info" %}
For teams running their own Yellowstone gRPC server, replay must be enabled server-side by setting `replay_stored_slots` to a value greater than `0`.
{% endhint %}

From v13.1.0 the Rust client handles reconnect automatically. It tracks the last fully processed slot, reconnects with `from_slot` set to that slot, deduplicates replayed messages, and falls back to a live subscription if the slot is outside the replay window. Auto-reconnect is disabled by default: omit `set_reconnect_config` to keep the original single-stream behaviour.

```rust
let client = GeyserGrpcClient::build_from_shared(endpoint)?
    .x_token(x_token)?
    .tls_config(ClientTlsConfig::new().with_native_roots())?
    .set_reconnect_config(ReconnectConfig::default())
    .connect()
    .await?;

let mut stream = client.subscribe_once(request).await?;

while let Some(msg) = stream.next().await {
    // stream transparently reconnects on disconnect,
    // replays from last processed slot, and deduplicates replayed messages
}
```

## Excluded programs

Light Protocol / ZK Compression (`compr6CUsB5m2jS4Y3831ztGSTnDpnKJTKS95d64XVq`) is excluded from all Dragon's Mouth gRPC streams and from `getProgramAccounts`. Each update produces a \~10MB blob, several gigabits per second of data, which is impractical to include in standard streams. To consume ZK Compression data, use the [Photon indexer](https://github.com/helius-labs/photon), which most users want for the processed end result rather than the raw account blobs.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
