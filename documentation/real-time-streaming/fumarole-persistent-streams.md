---
description: Scalable and reliable streaming of account and transaction data.
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
    visible: false
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# Fumarole persistent streams

Fumarole is a persistent gRPC streaming layer purpose-built for indexers, analytics, compliance, and any pipeline where missing a block is not an option. On top of Dragon's Mouth it adds server-side cursor tracking with 4 days of rolling retention, multi-node aggregation with automatic failover, and an at-least-once delivery guarantee, so you can disconnect and resume exactly where you left off and backfill anything you missed.

## Fumarole provides

* **High availability**: by collecting data from multiple downstream Solana nodes and merging them into a single stream, your stream does not get interrupted if a node restarts or is upgraded.
* **Persistence**: Fumarole stores up to 4 days of historical state and lets you reconnect your stream if your clients go down. Disconnect and resume from your last position any time within that window.
* **Parallel replay download :** Fumarole can replay in parallel geyser data wherever you left off.

## Use cases

Fumarole fits pipelines that value completeness and reliability over raw speed:

* **Indexers** that must process every block exactly once.
* **Analytics and accounting** systems that cannot tolerate gaps in the data.
* **Compliance and monitoring** tools that need a complete, ordered record.

## What not to use Fumarole for

Latency-first workloads. If you need data as fast as possible for trading or MEV and can tolerate missing the occasional slot, use [Dragon's Mouth gRPC](dragon-s-mouth-grpc) instead: it is the lowest-latency source. Fumarole trades a small amount of latency for completeness and resumability.

## Methods

Fumarole is a gRPC service. You manage a **persistent subscriber** (a consumer group) and stream from it:

| Method | What it does |
| --- | --- |
| `CreateConsumerGroup` / `CreateStaticConsumerGroup` | Create a persistent subscriber that tracks your position server-side. |
| `ListConsumerGroups` / `GetConsumerGroupInfo` / `DeleteConsumerGroup` | Manage your persistent subscribers. |
| `Subscribe` / `SubscribeV2` / `SubscribeData` | Stream account and transaction updates from a consumer group. |
| `DownloadBlock` / `DownloadBlockDataShard` | Replay a specific block, in parallel shards. |
| `GetChainTip` / `GetSlotRange` / `Version` | Inspect the stream's current tip, available slot range, and version. |

## Regional endpoints

Fumarole runs as **independent regional clusters**. We currently operate:

| Region | Endpoint          | Location  |
| ------ | ----------------- | --------- |
| EU     | `ams.rpcpool.com` | Amsterdam |
| US     | `nyc.rpcpool.com` | New York  |

**Choosing an endpoint**

For Fumarole, connect directly to a **regional endpoint** rather than shared `*.mainnet.rpcpool.com` endpoints. Pick the region closest to your backend infrastructure to minimise latency.

{% hint style="warning" %}
Shared `*.mainnet.rpcpool.com` endpoints are **not recommended for Fumarole**. See the next section for why.
{% endhint %}

**Why direct regional endpoints matter for Fumarole**

The shared endpoints (`*.mainnet.rpcpool.com`) routes traffic to the closest regional load balancer based on GeoDNS. For stateless RPC calls this is ideal, any region can serve any request.

**Persistent subscribers in Fumarole are stateful per cluster.** A persistent subscriber and the slot offsets it tracks live locally on the cluster where it was created and **do not replicate across regions**.

If you connect through a shared endpoint, your traffic can be routed to a region where your persistent subscriber does not exist - for example, after a routing change, a network event, or a shift in the perceived geography of your backend. When that happens, your subscriber will not be found on the new cluster and you will need to recreate it.

Pointing your Fumarole client directly at a regional endpoint avoids this entirely.

**Switching regions**

To move a persistent subscriber from one regional cluster to another (e.g., EU → US):

1. Recreate your persistent subscriber on the new cluster - subscribers do not carry over between regional clusters.
2. Point your client to the new regional endpoint.
3. Reconnect. Your existing token continues to work; no token changes are required.

**Cross-region redundancy (customer-managed)**

With multiple regional clusters available, you can implement cross-region redundancy on the client side. If one cluster experiences a major issue, you can fail over to another region by recreating the persistent subscriber **from the last slot you observed on the failing cluster**.

Fumarole makes this type of redundancy easy to handle in the client since it tracks the last **full** slot you consumed.

This pattern is fully customer-managed:

* Your client tracks the last-seen slot per cluster.
* Your client detects the failure and triggers the cutover.
* Triton does not synchronise subscriber state, track last-seen slots, or orchestrate failover between clusters.

If you need this level of redundancy, plan your slot bookkeeping and failover logic accordingly. For the full step-by-step detection, failover, and failback procedure, follow the [Fumarole cluster failover guide](https://kate-6.gitbook.io/triton-one-docs-v5/guides/solana/streaming/fumarole-cluster-failover).

## How to get started

Fumarole works with your existing mainnet subscription token, no separate access request needed.

**1. Pick a regional endpoint.** Connect directly to the cluster closest to your backend (see [Regional endpoints](#regional-endpoints) above): `ams.rpcpool.com` (EU) or `nyc.rpcpool.com` (US). Persistent subscribers are stateful per cluster, so avoid the shared `*.mainnet.rpcpool.com` endpoints.

**2. Authenticate with your token.** Pass your existing mainnet token as the `x-token`. No separate access request.

**3. Install a client.** The Fume CLI is the quickest way to try Fumarole and to create a persistent subscriber; the Rust and TypeScript SDKs are how you build.

{% tabs %}
{% tab title="Fume CLI" %}
```bash
cargo install yellowstone-fumarole-cli
```
{% endtab %}

{% tab title="Rust" %}
```toml
[dependencies]
yellowstone-fumarole-client = "0.5"
```
{% endtab %}

{% tab title="TypeScript" %}
```bash
npm install @triton-one/yellowstone-fumarole
```
{% endtab %}
{% endtabs %}

**4. Create a persistent subscriber, then stream from it.** The subscriber tracks your position server-side, so reconnecting with the same name resumes where you left off.

{% tabs %}
{% tab title="Rust" %}
```rust
use yellowstone_fumarole_client::{FumaroleClient, config::FumaroleConfig};
use yellowstone_grpc_proto::geyser::{SubscribeRequest, SubscribeRequestFilterTransactions};
use std::collections::HashMap;
use tokio_stream::StreamExt;

#[tokio::main]
async fn main() {
    let config = FumaroleConfig {
        endpoint: "https://ams.rpcpool.com".to_string(),
        x_token: Some("<your-token>".to_string()),
        ..Default::default()
    };

    let mut client = FumaroleClient::connect(config).await.expect("connect");

    let request = SubscribeRequest {
        transactions: HashMap::from([(
            "f1".to_owned(),
            SubscribeRequestFilterTransactions::default(),
        )]),
        ..Default::default()
    };

    // "my-subscriber" is a persistent subscriber you created (e.g. with the Fume CLI).
    let subscription = client.subscribe("my-subscriber".to_string(), request).await.expect("subscribe");
    let (_, source) = subscription.split();
    let mut source = source.like_dragonsmouth();

    while let Some(update) = source.next().await {
        println!("{:?}", update.expect("event"));
    }
}
```
{% endtab %}

{% tab title="TypeScript" %}
```typescript
import {
  FumaroleClient,
  SubscribeRequest,
  CommitmentLevel,
} from "@triton-one/yellowstone-fumarole";

const client = await FumaroleClient.connect({
  endpoint: "https://ams.rpcpool.com",
  xToken: "<your-token>",
  maxDecodingMessageSizeBytes: 100 * 1024 * 1024,
});

// Create the persistent subscriber once; reuse the name to resume.
const subscriberName = "my-subscriber";
await client.createPersistentSubscriber(subscriberName);

const request: SubscribeRequest = {
  commitment: CommitmentLevel.PROCESSED,
  accounts: {},
  transactions: { f1: { accountInclude: [], accountExclude: [], accountRequired: [] } },
  slots: {},
  transactionsStatus: {},
  blocks: {},
  blocksMeta: {},
  entry: {},
  accountsDataSlice: [],
};

const { source } = await client.dragonsmouthSubscribeWithConfig(subscriberName, request, {
  concurrentDownloadLimit: 1,
  commitInterval: 5000,
});

await source.forEach((update) => console.log(update));
```
{% endtab %}
{% endtabs %}

For the full Fume CLI walkthrough, read the [launch post](https://blog.triton.one/introducing-yellowstone-fumarole); for the complete API, see the [Rust and TypeScript SDKs](https://github.com/rpcpool/yellowstone-fumarole).

## Migrating from Dragon's Mouth

If you already have code built for our gRPC streams in Dragon's Mouth, integrating with Fumarole for additional reliability is easy. The code changes should be minimal as Fumarole uses the same types as Dragon's Mouth.

The main difference is that you need to manage a **persistent subscriber**, and alter your subscribe request slightly.

For more details see the Github repo:

[Yellowstone-fumarole-client crates.io](https://docs.rs/yellowstone-fumarole-client/0.5.0+solana.3/yellowstone_fumarole_client/)

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
