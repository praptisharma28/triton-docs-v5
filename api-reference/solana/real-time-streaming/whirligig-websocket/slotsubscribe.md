---
description: Subscribe to notifications when the validator processes a new slot.
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

# slotSubscribe

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "slotSubscribe"
}'
```
{% endtab %}

{% tab title="Kit" %}
```typescript
import { createSolanaRpcSubscriptions } from "@solana/kit";

const rpc = createSolanaRpcSubscriptions("wss://api.devnet.solana.com");

const subscription = await rpc
  .slotNotifications()
  .subscribe({ abortSignal: AbortSignal.timeout(5_000) });

for await (const notification of subscription) {
  console.log(notification);
}
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import { Connection } from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

const subscriptionId = connection.onSlotChange((slotInfo) => {
  console.log("Slot info:", slotInfo);
});
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use futures::StreamExt;
use solana_client::nonblocking::pubsub_client::PubsubClient;

#[tokio::main]
async fn main() -> Result<()> {
    let pubsub_client = PubsubClient::new("wss://api.devnet.solana.com/").await?;

    let (mut notifications, unsubscribe) = pubsub_client.slot_subscribe().await?;

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

**None**

## Response

```json
{
  "jsonrpc": "2.0",
  "result": 0,
  "id": 1
}
```

## Result

Subscription id. Pass this to [slotUnsubscribe](https://solana.com/docs/rpc/websocket/slotunsubscribe).

***

_Adapted from the_ [_Solana `slotSubscribe` reference_](https://solana.com/docs/rpc/websocket/slotsubscribe)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
