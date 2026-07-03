---
description: Subscribe to notifications when an account's lamports or data change.
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

# accountSubscribe

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "accountSubscribe",
  "params": [
    "CM78CPUeXjn8o3yroDHxUtKsZZgoy4GPkPPXfouKNH12",
    {
      "encoding": "jsonParsed",
      "commitment": "finalized"
    }
  ]
}'
```
{% endtab %}

{% tab title="Kit" %}
```typescript
import { address, createSolanaRpcSubscriptions } from "@solana/kit";

const rpc = createSolanaRpcSubscriptions("wss://api.devnet.solana.com");

const subscription = await rpc
  .accountNotifications(
    address("So11111111111111111111111111111111111111112"),
    {
      encoding: "jsonParsed",
      commitment: "finalized"
    }
  )
  .subscribe({ abortSignal: AbortSignal.timeout(50_000) });

for await (const notification of subscription) {
  console.log(notification);
}
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import { Connection, PublicKey } from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

const publicKey = new PublicKey("CM78CPUeXjn8o3yroDHxUtKsZZgoy4GPkPPXfouKNH12");

const subscriptionId = connection.onAccountChange(
  publicKey,
  (accountInfo, context) => {
    console.log("Account changed:", accountInfo);
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
use solana_client::{nonblocking::pubsub_client::PubsubClient, rpc_config::RpcAccountInfoConfig};
use solana_commitment_config::CommitmentConfig;
use solana_sdk::pubkey::Pubkey;

#[tokio::main]
async fn main() -> Result<()> {
    let pubsub_client = PubsubClient::new("wss://api.devnet.solana.com/").await?;

    // using WSOL address
    let pubkey = Pubkey::from_str_const(&"So11111111111111111111111111111111111111112");

    let config = RpcAccountInfoConfig {
        commitment: Some(CommitmentConfig::confirmed()),
        encoding: None,
        data_slice: None,
        min_context_slot: None,
    };

    let (mut notifications, unsubscribe) = pubsub_client
        .account_subscribe(&pubkey, Some(config))
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

**`0`** · string !required

Account Pubkey, as base-58 encoded string

**`1`** · object

Optional configuration object.

**`commitment`** · string !values processed confirmed finalized !default finalized

The commitment describes how finalized a block is at that point in time. See [Configuring State Commitment](https://solana.com/docs/rpc#configuring-state-commitment).

**`encoding`** · string !values base58 base64 base64+zstd binary jsonParsed !default binary

Encoding format for account data.

* `base58` is slow.
* `jsonParsed` attempts to use program-specific parsers to return more human-readable account state data.
* If `jsonParsed` is requested but no parser is available, the RPC node falls back to `[data, "base64"]`.
* `binary` is a deprecated legacy alias for base58 data returned as a plain string.

**`dataSlice`** · object

Request a slice of the account's data.

| Field    | Type    | Description                                           |
| -------- | ------- | ----------------------------------------------------- |
| `offset` | `usize` | Byte offset from which to start reading account data. |
| `length` | `usize` | Number of bytes to return.                            |

{% hint style="info" %}
Data slicing is only available for `base58`, `base64`, `base64+zstd`, and `binary` encodings.
{% endhint %}

{% hint style="info" %}
`base58` and `binary` use base-58 encoding and fail if the selected payload is more than 128 bytes after slicing. Use `base64` for larger accounts.
{% endhint %}

{% hint style="info" %}
`accountSubscribe` accepts the same config fields as [getAccountInfo](https://solana.com/docs/rpc/http/getaccountinfo), but PubSub subscriptions currently ignore `minContextSlot`.
{% endhint %}

## Response

```json
{
  "jsonrpc": "2.0",
  "result": 23784,
  "id": 1
}
```

## Result

Subscription id. Pass this to [accountUnsubscribe](https://solana.com/docs/rpc/websocket/accountunsubscribe).

***

_Adapted from the_ [_Solana `accountSubscribe` reference_](https://solana.com/docs/rpc/websocket/accountsubscribe)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
