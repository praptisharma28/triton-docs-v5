---
description: Subscribe to status notifications for a transaction signature.
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

# signatureSubscribe

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "signatureSubscribe",
  "params": [
    "2EBVM6cB8vAAD93Ktr6Vd8p67XPbQzCJX47MpReuiCXJAtcjaxpvWpcg9Ege1Nr5Tk3a2GFrByT7WPBjdsTycY9b",
    {
      "commitment": "finalized",
      "enableReceivedNotification": false
    }
  ]
}'
```
{% endtab %}

{% tab title="Kit" %}
```typescript
import { createSolanaRpcSubscriptions, signature } from "@solana/kit";

const rpc = createSolanaRpcSubscriptions("wss://api.devnet.solana.com");

const sig = signature(
  "2EBVM6cB8vAAD93Ktr6Vd8p67XPbQzCJX47MpReuiCXJAtcjaxpvWpcg9Ege1Nr5Tk3a2GFrByT7WPBjdsTycY9b"
);

const subscription = await rpc
  .signatureNotifications(sig, {
    commitment: "finalized",
    enableReceivedNotification: false
  })
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

const signature =
  "2EBVM6cB8vAAD93Ktr6Vd8p67XPbQzCJX47MpReuiCXJAtcjaxpvWpcg9Ege1Nr5Tk3a2GFrByT7WPBjdsTycY9b";

const subscriptionId = connection.onSignature(
  signature,
  (signatureResult, context) => {
    console.log("Signature result:", signatureResult);
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
    rpc_config::RpcSignatureSubscribeConfig,
};
use solana_commitment_config::CommitmentConfig;
use solana_sdk::signature::Signature;
use std::str::FromStr;

#[tokio::main]
async fn main() -> Result<()> {
    let pubsub_client = PubsubClient::new("wss://api.devnet.solana.com/").await?;

    let signature = Signature::from_str(
        "2EBVM6cB8vAAD93Ktr6Vd8p67XPbQzCJX47MpReuiCXJAtcjaxpvWpcg9Ege1Nr5Tk3a2GFrByT7WPBjdsTycY9b",
    )?;

    let config = RpcSignatureSubscribeConfig {
        commitment: Some(CommitmentConfig::finalized()),
        enable_received_notification: Some(false),
    };

    let (mut notifications, unsubscribe) = pubsub_client
        .signature_subscribe(&signature, Some(config))
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

**`signature`** · string · required

Transaction signature, as a base-58 encoded string.

{% hint style="info" %}
The signature must be the first signature from the transaction.
{% endhint %}

**`config`** · object

Optional configuration object.

**`commitment`** · string · default `finalized`

Values: `processed`, `confirmed`, `finalized`
The commitment describes how finalized a block is at that point in time. See [Configuring State Commitment](https://solana.com/docs/rpc#configuring-state-commitment).

**`enableReceivedNotification`** · boolean · default `false`

Values: `true`, `false`
Whether to also notify when the RPC first receives the signature, before it reaches the requested commitment.

## Response

```json
{
  "jsonrpc": "2.0",
  "result": 0,
  "id": 1
}
```

## Result

Subscription id. Pass this to [signatureUnsubscribe](https://solana.com/docs/rpc/websocket/signatureunsubscribe) if you cancel the subscription before it fires.

***

_Adapted from the_ [_Solana `signatureSubscribe` reference_](https://solana.com/docs/rpc/websocket/signaturesubscribe)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
