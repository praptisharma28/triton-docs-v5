---
description: >-
  Move a Dragon's Mouth gRPC subscriber to Fumarole for persistence,
  resumability, and parallel connections.
---

# How to migrate from Dragon's Mouth to Fumarole

Fumarole serves the same Solana data as Dragon's Mouth, with a persistence and delivery layer in front of it: persistent subscribers you can resume after a disconnect, parallel gRPC connections that avoid head-of-line blocking, and automatic reconnect. The client API is close to `yellowstone-grpc`, so most of the work is swapping the client and choosing how you consume the stream.

## What changes

|             | Dragon's Mouth (`yellowstone-grpc`)        | Fumarole (`yellowstone-fumarole-client`)              |
| ----------- | ------------------------------------------ | ----------------------------------------------------- |
| Connections | one gRPC stream                            | multiple gRPC connections, avoids HoL blocking        |
| Subscriber  | per-session                                | persistent, reusable across sessions                  |
| Disconnects | client tracks the slot and replays         | resume from your committed position                   |
| Best for    | lowest-latency live feeds                  | never-miss workloads: indexing, accounting, analytics |

## Connect to Fumarole

Add the [`yellowstone-fumarole-client`](https://docs.rs/yellowstone-fumarole-client) crate and build a client from a config. The preferred form is a YAML file read with `serde_yaml`:

```yaml
endpoint: https://fumarole.endpoint.rpcpool.com
x-token: <your-token>
response_compression: zstd
```

{% tabs %}
{% tab title="Rust" %}
```rust
use yellowstone_fumarole_client::FumaroleClient;
use yellowstone_fumarole_client::config::FumaroleConfig;

let config: FumaroleConfig =
    serde_yaml::from_reader(std::fs::File::open("config.yaml")?)?;
let mut client = FumaroleClient::connect(config).await.expect("connect");
```
{% endtab %}
{% endtabs %}

## Fastest path: the Dragon's Mouth adapter

For the smallest change, `dragonsmouth_subscribe` mimics the Dragon's Mouth API, so your existing `SubscribeRequest` and receive loop keep working, now against a named persistent subscriber:

{% tabs %}
{% tab title="Rust" %}
```rust
let session = client.dragonsmouth_subscribe(subscriber_name, request).await?;
let DragonsmouthAdapterSession { sink: _, mut source, mut fumarole_handle } = session;
// consume `source` exactly like a Dragon's Mouth stream
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
The Dragon's Mouth adapter is **deprecated**. Use it to get running quickly, then move to the native API below.
{% endhint %}

## Recommended: the native Fumarole API

The native API streams from a named persistent subscriber and lets you process in slot order. Use `slot_sequential()` for slot-ordered work:

{% tabs %}
{% tab title="Rust" %}
```rust
use futures::StreamExt;
use yellowstone_fumarole_client::stream::FumaroleEvent;

let subscription = client.subscribe(subscriber_name, request).await?;
let (_sink, stream) = subscription.split();
let mut slot_stream = stream.slot_sequential();

while let Some(item) = slot_stream.next().await {
    match item? {
        FumaroleEvent::Data { slot, .. } => { /* process the update */ }
        FumaroleEvent::SlotEnded(slot) => { /* slot complete */ }
    }
}
```
{% endtab %}
{% endtabs %}

For block-oriented processing, use `stream.block_stream()` and match `FumaroleBlockStreamEvent::Block` and `::SlotStatus` instead.

## Scale for high traffic

For heavy or high-latency workloads, open parallel data-plane connections and enable Zstd. Set `response_compression: zstd` in the config (above), then pass a subscribe config:

{% tabs %}
{% tab title="Rust" %}
```rust
let subscribe_config = FumaroleSubscribeConfig {
    num_data_plane_tcp_connections: NonZeroU8::new(4).unwrap(), // up to 4
    ..Default::default()
};
let subscription = client
    .subscribe_with_config(subscriber_name, request, subscribe_config)
    .await?;
```
{% endtab %}
{% endtabs %}

## Control when progress commits

The subscriber auto-commits its position by default. For at-least-once processing, set `auto_commit: false` and commit only after your slot work succeeds:

{% tabs %}
{% tab title="Rust" %}
```rust
let subscribe_config = FumaroleSubscribeConfig { auto_commit: false, ..Default::default() };
// ...
FumaroleEvent::SlotEnded(slot) => {
    slot_stream.commit(); // commit after processing this slot succeeds
}
```
{% endtab %}
{% endtabs %}

## What's next

* [Fumarole cluster failover](fumarole-cluster-failover) for cross-region high availability.
* [`yellowstone-fumarole-client` on docs.rs](https://docs.rs/yellowstone-fumarole-client) for the full API, feature flags, and runnable examples.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
