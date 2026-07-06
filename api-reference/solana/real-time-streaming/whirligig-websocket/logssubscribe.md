---
description: Subscribe to transaction log messages that match a log filter.
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

# logsSubscribe

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "logsSubscribe",
  "params": [
    {
      "mentions": ["11111111111111111111111111111111"]
    },
    {
      "commitment": "finalized"
    }
  ]
}'
```
{% endtab %}

{% tab title="Kit" %}
```typescript
import { createSolanaRpcSubscriptions } from "@solana/kit";

const rpc = createSolanaRpcSubscriptions("wss://api.devnet.solana.com");

const subscription = await rpc
  .logsNotifications(
    { mentions: ["11111111111111111111111111111111"] },
    { commitment: "confirmed" }
  )
  .subscribe({ abortSignal: AbortSignal.timeout(50_000) });

for await (const notification of subscription) {
  console.log(notification);
}
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import { Connection } from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

const subscriptionId = connection.onLogs(
  "11111111111111111111111111111111",
  (logs, context) => {
    console.log("Logs:", logs);
    console.log("Context:", context);
  },
  "finalized"
);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use futures::StreamExt;
use solana_client::{
    nonblocking::pubsub_client::PubsubClient,
    rpc_config::{RpcTransactionLogsConfig, RpcTransactionLogsFilter},
};
use solana_commitment_config::CommitmentConfig;

#[tokio::main]
async fn main() -> Result<()> {
    let pubsub_client = PubsubClient::new("wss://api.devnet.solana.com/").await?;

    let filter = RpcTransactionLogsFilter::Mentions(vec![
        "11111111111111111111111111111111".to_string(),
    ]);

    let config = RpcTransactionLogsConfig {
        commitment: Some(CommitmentConfig::finalized()),
    };

    let (mut notifications, unsubscribe) = pubsub_client
        .logs_subscribe(filter, config)
        .await?;

    while let Some(notification) = notifications.next().await {
        println!("{:?}", notification);
    }

    unsubscribe().await;

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`filter`** · string | object · required

Filter describing which transactions should emit log notifications.

* `all` subscribes to all transactions except simple vote transactions
* `allWithVotes` subscribes to all transactions, including simple vote transactions
* `{ "mentions": [<pubkey>] }` subscribes to transactions that mention a single base-58 encoded address

{% hint style="info" %}
The `mentions` filter currently supports exactly one address. Listing more than one returns an `Invalid params` error.
{% endhint %}

**`config`** · object

Optional configuration object.

**`commitment`** · string · default `finalized`

Values: `processed`, `confirmed`, `finalized`
The commitment describes how finalized a block is at that point in time. See [Configuring State Commitment](https://solana.com/docs/rpc#configuring-state-commitment).

## Response

```json
{
  "jsonrpc": "2.0",
  "result": 24040,
  "id": 1
}
```

## Result

Subscription id. Pass this to [logsUnsubscribe](https://solana.com/docs/rpc/websocket/logsunsubscribe).

***

_Adapted from the_ [_Solana `logsSubscribe` reference_](https://solana.com/docs/rpc/websocket/logssubscribe)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
