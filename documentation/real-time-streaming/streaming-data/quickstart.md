---
description: Open your first Triton stream and see live Solana data in minutes.
---

# Quickstart

In this quickstart we'll show you how to stream live Solana data, using any of Triton's four streaming services:

1. **Dragon's Mouth gRPC.** Sub-slot real-time updates for accounts, transactions, slots, and blocks over gRPC. The lowest-latency path, best for backends.
2. **Whirligig WebSockets.** A drop-in for native Solana WebSockets, backed by gRPC. The fastest real-time data for browsers and frontends.
3. **Fumarole reliable streams.** A redundant streaming layer with 4 days of stored data and cursor resume, for pipelines that can't miss a block.
4. **Deshred transactions.** Pre-execution transactions reconstructed from raw shreds. The earliest intent signal for traders.

## 0. Prerequisites

All streaming services we'll be testing use Triton endpoints with token-based authentication. Before starting, here's what you need:

* An active Triton subscription
* Your endpoint URL and secret token from the [customer dashboard](https://customers.triton.one/)
* An environment in TypeScript, Rust, Go, or Python

## 1. Install

Set up a fresh project for the service you want to try.

{% tabs %}
{% tab title="Dragon's Mouth" %}
{% tabs %}
{% tab title="TypeScript" %}
```bash
mkdir solana-grpc-stream && cd solana-grpc-stream
npm init -y
npm install @triton-one/yellowstone-grpc
npm install --save-dev typescript ts-node @types/node
npx tsc --init
```
{% endtab %}

{% tab title="Rust" %}
```toml
[dependencies]
yellowstone-grpc-client = "13"
yellowstone-grpc-proto = "12"
tokio = { version = "1", features = ["full"] }
futures = "0.3"
tonic = { version = "0.14", features = ["tls"] }
```
{% endtab %}

{% tab title="Go" %}
```bash
mkdir solana-grpc-stream && cd solana-grpc-stream
go mod init example.com/solana-grpc-stream
go get github.com/rpcpool/yellowstone-grpc/examples/golang@latest
```
{% endtab %}
{% endtabs %}

Other languages, see the [yellowstone-grpc/examples](https://github.com/rpcpool/yellowstone-grpc/tree/master/examples) directory.
{% endtab %}

{% tab title="Whirligig" %}
Whirligig speaks the standard Solana WebSocket protocol. TypeScript uses the `ws` package; Rust uses Solana's `PubsubClient` from `solana-client`.

{% tabs %}
{% tab title="TypeScript" %}
```bash
mkdir whirligig-stream && cd whirligig-stream
npm init -y
npm install ws
npm install --save-dev typescript ts-node @types/node @types/ws
npx tsc --init
```
{% endtab %}

{% tab title="Rust" %}
```toml
[dependencies]
solana-client = "2"
solana-pubkey = "2"
solana-account-decoder-client-types = "2"
solana-rpc-client-api = "2"
solana-sdk = "2"
tokio = { version = "1", features = ["full"] }
futures = "0.3"
```
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="Fumarole" %}
Fumarole adds a persistent-subscriber layer on top of Dragon's Mouth.

{% tabs %}
{% tab title="TypeScript" %}
```bash
mkdir fumarole-stream && cd fumarole-stream
npm init -y
npm install @triton-one/yellowstone-fumarole
npm install --save-dev typescript ts-node @types/node
npx tsc --init
```
{% endtab %}

{% tab title="Rust" %}
```toml
[dependencies]
yellowstone-fumarole-client = "0.6"
yellowstone-grpc-proto = "12"
tokio = { version = "1", features = ["full"] }
futures = "0.3"
```
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="Deshred" %}
Deshred is a separate gRPC method on the same `yellowstone-grpc` service. Same install as Dragon's Mouth.

{% tabs %}
{% tab title="TypeScript" %}
```bash
mkdir deshred-stream && cd deshred-stream
npm init -y
npm install @triton-one/yellowstone-grpc
npm install --save-dev typescript ts-node @types/node
npx tsc --init
```
{% endtab %}

{% tab title="Rust" %}
```toml
[dependencies]
yellowstone-grpc-client = "13"
yellowstone-grpc-proto = "12"
solana-signature = "2"
tokio = { version = "1", features = ["full"] }
futures = "0.3"
tonic = { version = "0.14", features = ["tls"] }
anyhow = "1"
```
{% endtab %}
{% endtabs %}
{% endtab %}
{% endtabs %}

## 2. Connect and subscribe

{% tabs %}
{% tab title="Dragon's Mouth" %}
**What we're doing:** subscribe to all writes on a specific account (the USDC mint) and log each Protobuf-decoded update.

First, initialise the client and connect:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import Client from "@triton-one/yellowstone-grpc";

const client = new Client(
  "https://<your-endpoint>.mainnet.rpcpool.com",
  "<your-token>",
  undefined
);

await client.connect();

const version = await client.getVersion();
console.log(version);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use {
    tonic::transport::channel::ClientTlsConfig,
    yellowstone_grpc_client::GeyserGrpcClient,
};

#[tokio::main]
async fn main() -> anyhow::Result<()> {
    let endpoint = "https://<your-endpoint>.mainnet.rpcpool.com:443";
    let x_token = "<your-token>";

    let mut client = GeyserGrpcClient::build_from_shared(endpoint)?
        .x_token(Some(x_token))?
        .tls_config(ClientTlsConfig::new().with_native_roots())?
        .connect()
        .await?;

    let version = client.get_version().await?;
    println!("{:?}", version);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

Then open a subscription stream, attach a data handler, and send the subscribe request:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import { CommitmentLevel, SubscribeRequest } from "@triton-one/yellowstone-grpc";

const stream = await client.subscribe();

stream.on("data", (data) => {
  console.log("data", data);
});

const request: SubscribeRequest = {
  slots: { slots: {} },
  accounts: {
    "usdc": {
      account: ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v"],
      owner: [],
      filters: [],
    },
  },
  transactions: {},
  transactionsStatus: {},
  entry: {},
  blocks: {},
  blocksMeta: {},
  accountsDataSlice: [],
  commitment: CommitmentLevel.CONFIRMED,
  ping: undefined,
};

await new Promise<void>((resolve, reject) => {
  stream.write(request, (err) => {
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
```
{% endtab %}

{% tab title="Rust" %}
```rust
use {
    futures::stream::StreamExt,
    std::collections::HashMap,
    yellowstone_grpc_proto::prelude::{
        CommitmentLevel, SubscribeRequest, SubscribeRequestFilterAccounts,
        SubscribeRequestFilterSlots,
    },
};

let mut accounts = HashMap::new();
accounts.insert(
    "usdc".to_string(),
    SubscribeRequestFilterAccounts {
        account: vec!["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v".to_string()],
        owner: vec![],
        filters: vec![],
        ..Default::default()
    },
);

let mut slots = HashMap::new();
slots.insert("client".to_string(), SubscribeRequestFilterSlots::default());

let request = SubscribeRequest {
    accounts,
    slots,
    commitment: Some(CommitmentLevel::Confirmed.into()),
    ..Default::default()
};

let (_sink, mut stream) = client.subscribe_with_request(Some(request)).await?;

while let Some(message) = stream.next().await {
    println!("data: {:?}", message?);
}
```
{% endtab %}

{% tab title="grpcurl" %}
```shell
./grpcurl \
  -proto geyser.proto \
  -d '{"slots": { "slots": {} }, "accounts": { "usdc": { "account": ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v"] } }, "transactions": {}, "blocks": {}, "blocks_meta": {}}' \
  -H "x-token: <token>" \
  api.rpcpool.com:443 \
  geyser.Geyser/Subscribe
```
{% endtab %}
{% endtabs %}

For Go and other languages: [yellowstone-grpc/examples](https://github.com/rpcpool/yellowstone-grpc/tree/master/examples).
{% endtab %}

{% tab title="Whirligig" %}
**What we're doing:** open a Solana-standard WebSocket connection to Whirligig and subscribe to changes on the system clock sysvar.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import WebSocket from "ws";

const ws = new WebSocket(
  "wss://<your-endpoint>.mainnet.rpcpool.com/<your-token>/whirligig"
);

ws.on("open", () => {
  ws.send(
    JSON.stringify({
      jsonrpc: "2.0",
      id: 1,
      method: "accountSubscribe",
      params: [
        "SysvarC1ock11111111111111111111111111111111",
        { commitment: "processed", encoding: "base58" },
      ],
    })
  );
});

ws.on("message", (data) => {
  console.log("Update:", JSON.parse(data.toString()));
});
```
{% endtab %}

{% tab title="Rust" %}
```rust
use {
    futures::StreamExt,
    solana_account_decoder_client_types::UiAccountEncoding,
    solana_client::nonblocking::pubsub_client::PubsubClient,
    solana_pubkey::Pubkey,
    solana_rpc_client_api::config::RpcAccountInfoConfig,
    solana_sdk::commitment_config::CommitmentConfig,
    std::str::FromStr,
};

#[tokio::main]
async fn main() -> anyhow::Result<()> {
    let endpoint =
        "wss://<your-endpoint>.mainnet.rpcpool.com/<your-token>/whirligig";

    let client = PubsubClient::new(endpoint).await?;

    let pubkey = Pubkey::from_str("SysvarC1ock11111111111111111111111111111111")?;
    let config = RpcAccountInfoConfig {
        encoding: Some(UiAccountEncoding::Base58),
        commitment: Some(CommitmentConfig::processed()),
        ..Default::default()
    };

    let (mut stream, _unsubscribe) =
        client.account_subscribe(&pubkey, Some(config)).await?;

    while let Some(account) = stream.next().await {
        println!("Update: {:?}", account);
    }

    Ok(())
}
```
{% endtab %}
{% endtabs %}

Whirligig speaks the [standard Solana WebSocket API](https://docs.solana.com/api/websocket) plus an extended `transactionSubscribe` method. Full method reference: Whirligig WebSocket API.
{% endtab %}

{% tab title="Fumarole" %}
**What we're doing:** connect to a regional Fumarole endpoint, create a persistent subscriber, and stream all account writes for the SPL Token program. Reconnecting with the same subscriber name resumes from where the cursor left off.

Connect to a regional endpoint (`ams.rpcpool.com` for Europe, `nyc.rpcpool.com` for the US):

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import {
  FumaroleClient,
  InitialOffsetPolicy,
} from "@triton-one/yellowstone-fumarole";
import { CommitmentLevel, SubscribeRequest } from "@triton-one/yellowstone-grpc";

const TOKEN_ADDRESS = "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA";
const subscriberName = "helloworld";

const client = await FumaroleClient.connect({
  endpoint: process.env.FUMAROLE_ENDPOINT!, // e.g. https://ams.rpcpool.com
  xToken: process.env.FUMAROLE_X_TOKEN!,
  maxDecodingMessageSizeBytes: 100 * 1024 * 1024,
});
```
{% endtab %}
{% endtabs %}

Build the subscribe request:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
const request: SubscribeRequest = {
  commitment: CommitmentLevel.PROCESSED,
  accounts: {
    token: {
      account: [],
      owner: [TOKEN_ADDRESS],
      filters: [],
    },
  },
  transactions: {
    token: {
      accountInclude: [TOKEN_ADDRESS],
      accountExclude: [],
      accountRequired: [],
    },
  },
  slots: { test: { filterByCommitment: true } },
  transactionsStatus: {},
  blocks: {},
  blocksMeta: {},
  entry: {},
  ping: { id: Date.now() },
  accountsDataSlice: [],
  fromSlot: undefined,
};
```
{% endtab %}
{% endtabs %}

Create the persistent subscriber and consume:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
await client.createPersistentSubscriber({
  consumerGroupName: subscriberName,
  initialOffsetPolicy: InitialOffsetPolicy.LATEST,
});

const { sink: _sink, source } = await client.dragonsmouthSubscribe(
  subscriberName,
  request
);

await source.forEach((update) => {
  console.log("Update:", update);
});
```
{% endtab %}

{% tab title="Rust" %}
```rust
use {
    futures::StreamExt,
    yellowstone_fumarole_client::{config::FumaroleConfig, FumaroleClient},
    yellowstone_grpc_proto::geyser::{
        CommitmentLevel, SubscribeRequest, SubscribeRequestFilterAccounts,
        subscribe_update::UpdateOneof,
    },
};

#[tokio::main]
async fn main() -> anyhow::Result<()> {
    let config = FumaroleConfig {
        endpoint: std::env::var("FUMAROLE_ENDPOINT")?,
        x_token: Some(std::env::var("FUMAROLE_X_TOKEN")?),
        ..Default::default()
    };
    let mut client = FumaroleClient::connect(config).await?;

    let mut accounts = std::collections::HashMap::new();
    accounts.insert(
        "token".to_string(),
        SubscribeRequestFilterAccounts {
            account: vec![],
            owner: vec!["TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA".into()],
            filters: vec![],
            ..Default::default()
        },
    );

    let request = SubscribeRequest {
        accounts,
        commitment: Some(CommitmentLevel::Processed.into()),
        ..Default::default()
    };

    let (_sink, mut source) = client
        .dragonsmouth_subscribe("helloworld", request)
        .await?;

    while let Some(update) = source.next().await {
        if let Some(UpdateOneof::Account(acc)) = update?.update_oneof {
            println!("Account update at slot {}", acc.slot);
        }
    }

    Ok(())
}
```
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="Deshred" %}
**What we're doing:** subscribe to the `SubscribeDeshred` gRPC stream, filter out vote transactions, and log every transaction signature with its slot.

Deshred delivers transactions reconstructed from shreds _**before**_ the node executes them, so you see intent earlier than with any other stream.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import Client, {
  SubscribeDeshredRequest,
} from "@triton-one/yellowstone-grpc";

const client = new Client(
  "https://<your-endpoint>.mainnet.rpcpool.com",
  "<your-token>",
  undefined
);

const stream = await client.subscribeDeshred();

stream.on("data", (data) => {
  console.log("data", data);
});

const request: SubscribeDeshredRequest = {
  deshredTransactions: {
    client: {
      vote: false,
      accountInclude: [],
      accountExclude: [],
      accountRequired: [],
    },
  },
  ping: undefined,
};

await new Promise<void>((resolve, reject) => {
  stream.write(request, (err) =>
    err ? reject(err) : resolve()
  );
});
```
{% endtab %}

{% tab title="Rust" %}
```rust
use {
    futures::{sink::SinkExt, stream::StreamExt},
    solana_signature::Signature,
    std::collections::HashMap,
    tonic::transport::channel::ClientTlsConfig,
    yellowstone_grpc_client::GeyserGrpcClient,
    yellowstone_grpc_proto::prelude::{
        subscribe_update_deshred::UpdateOneof, SubscribeDeshredRequest,
        SubscribeRequestFilterDeshredTransactions, SubscribeRequestPing,
    },
};

#[tokio::main]
async fn main() -> anyhow::Result<()> {
    let endpoint = std::env::var("ENDPOINT")
        .unwrap_or("https://<endpoint>".into());
    let x_token = std::env::var("X_TOKEN").ok();

    let mut client = GeyserGrpcClient::build_from_shared(endpoint)?
        .x_token(x_token)?
        .tls_config(ClientTlsConfig::new().with_native_roots())?
        .http2_adaptive_window(true)
        .initial_connection_window_size(8 * 1024 * 1024)
        .initial_stream_window_size(4 * 1024 * 1024)
        .connect()
        .await?;

    let request = SubscribeDeshredRequest {
        deshred_transactions: HashMap::from([(
            "deshred".into(),
            SubscribeRequestFilterDeshredTransactions {
                vote: Some(false),
                account_include: vec![],
                account_exclude: vec![],
                account_required: vec![],
            },
        )]),
        ping: None,
    };

    let (mut tx, mut stream) =
        client.subscribe_deshred_with_request(Some(request)).await?;

    while let Some(msg) = stream.next().await {
        match msg?.update_oneof {
            Some(UpdateOneof::DeshredTransaction(update)) => {
                let info = update.transaction.as_ref().unwrap();
                let sig = Signature::try_from(info.signature.as_slice())?;
                println!("slot={} sig={sig} vote={}", update.slot, info.is_vote);
            }
            Some(UpdateOneof::Ping(_)) => {
                tx.send(SubscribeDeshredRequest {
                    ping: Some(SubscribeRequestPing { id: 1 }),
                    ..Default::default()
                })
                .await?;
            }
            Some(UpdateOneof::Pong(_)) => {}
            None => break,
        }
    }

    Ok(())
}
```
{% endtab %}
{% endtabs %}
{% endtab %}
{% endtabs %}

## 3. Verify your stream

Run your code:

{% tabs %}
{% tab title="TypeScript" %}
```bash
npx ts-node main.ts
```
{% endtab %}

{% tab title="Rust" %}
```bash
cargo run
```
{% endtab %}

{% tab title="Go" %}
```bash
go run main.go
```
{% endtab %}
{% endtabs %}

Then walk through this checklist:

1. **Confirm the stream is open.** Your first matching event should arrive within 1-2 slots (\~800 ms). For Whirligig, you'll see a subscription-ID confirmation first: `{ "jsonrpc": "2.0", "result": 42, "id": 1 }`. For the rest of the services (running on gRPC), the client logs an active connection on `connect()` and events start arriving immediately.
2. **Watch matching events flow.** If nothing arrives after 5 seconds, your filter is probably too narrow.
3. **Keep long-lived connections alive.** Whirligig closes idle WebSocket connections after about 60 seconds, so send a `ping` periodically. gRPC clients handle transport keepalive automatically, but Dragon's Mouth also recommends periodic pings, since upstream providers like Cloudflare can close idle streams. See [Sending pings](dragon-s-mouth-grpc.md#sending-pings).
4. **Widen the filter to test, then tighten back.** Subscribe to all transactions or all writes for a busy program (e.g. SPL Token) to confirm data is reaching you. Once you see traffic, narrow the filter to what your app actually needs.

### Expected first message

{% tabs %}
{% tab title="Dragon's Mouth" %}
A Protobuf-decoded `SubscribeUpdate`. Each message has the matching filter labels and a payload type (`account`, `transaction`, `slot`, `block`, `blockMeta`, or `entry`). Account-update example:

```
data {
  filters: [ 'usdc' ],
  account: {
    account: {
      pubkey: <Buffer ...>,
      lamports: '2039280',
      owner: <Buffer ...>,
      executable: false,
      rentEpoch: '0',
      data: <Buffer ...>,
      writeVersion: '...'
    },
    slot: '275123456',
    isStartup: false
  }
}
```
{% endtab %}

{% tab title="Whirligig" %}
First, a subscription-ID confirmation:

```json
{ "jsonrpc": "2.0", "result": 42, "id": 1 }
```

Then JSON-RPC notifications matching the standard Solana WebSocket API. Account-notification example:

```json
{
  "jsonrpc": "2.0",
  "method": "accountNotification",
  "params": {
    "result": {
      "context": { "slot": 275123456 },
      "value": {
        "lamports": 1169280,
        "owner": "Sysvar1111111111111111111111111111111111111",
        "data": ["9AAG2zomYcUVKQfondw13yz193crQ5R5...", "base58"],
        "executable": false,
        "rentEpoch": 361
      }
    },
    "subscription": 42
  }
}
```
{% endtab %}

{% tab title="Fumarole" %}
A Protobuf `SubscribeUpdate` delivered through the persistent subscriber. The first event is whatever's at the cursor's offset (latest, in this example). Reconnecting with the same subscriber name resumes from the next slot after the last one you consumed:

```
Update: {
  transaction: {
    signature: <Buffer ...>,
    isVote: false,
    transaction: {...},
    meta: {...}
  },
  slot: '275123456'
}
```
{% endtab %}

{% tab title="Deshred" %}
One line per pre-execution transaction, with slot, signature, and vote flag. These transactions have not yet been executed by the validator, they may still fail or land on a dead fork. For status, logs, balance changes, or compute units, run a normal Dragon's Mouth `transactions` subscription alongside:

```
slot=275123456 sig=5j7s4Hk3...QmZ9xT vote=false
slot=275123456 sig=2pK8nLm9...VbY3wQ vote=false
slot=275123457 sig=8aRf6dGj...XwE5pN vote=false
```
{% endtab %}
{% endtabs %}

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-radio">:radio:</i> <strong>Dragon's Mouth gRPC</strong></td><td>Sub-slot real-time updates for accounts, transactions, slots, and blocks via gRPC.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/dragon-s-mouth-grpc">Dragon's Mouth gRPC</a></td></tr><tr><td><i class="fa-fire">:fire:</i> <strong>Deshred transactions</strong></td><td>Pre-execution transactions reconstructed from raw shreds. Earliest intent signal for traders.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/deshred-transactions">Deshred transactions</a></td></tr><tr><td><i class="fa-rotate-right">:rotate-right:</i> <strong>Whirligig WebSockets</strong></td><td>Drop-in for native Solana WebSockets. Fastest real-time data for frontends, backed by gRPC.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/whirligig-websockets">Whirligig WebSockets</a></td></tr><tr><td><i class="fa-layer-group">:layer-group:</i> <strong>Fumarole reliable streams</strong></td><td>Redundant streaming layer with 4 days of stored data and built-in cursor resume.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/fumarole-persistent-streams">Fumarole reliable streams</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
