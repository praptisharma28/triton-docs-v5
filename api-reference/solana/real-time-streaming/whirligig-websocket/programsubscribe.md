---
description: Subscribe to notifications for accounts owned by a given program.
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

# programSubscribe

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "programSubscribe",
  "params": [
    "11111111111111111111111111111111",
    {
      "encoding": "base64",
      "filters": [{ "dataSize": 80 }]
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
  .programNotifications(address("11111111111111111111111111111111"), {
    encoding: "base64",
    filters: [{ dataSize: 80n }]
  })
  .subscribe({ abortSignal: AbortSignal.timeout(5_000) });

for await (const notification of subscription) {
  console.log(notification);
}
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import { Connection, PublicKey } from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

const programId = new PublicKey("11111111111111111111111111111111");

const subscriptionId = connection.onProgramAccountChange(
  programId,
  (keyedAccountInfo, context) => {
    console.log("Account changed:", keyedAccountInfo);
    console.log("Context:", context);
  },
  "finalized",
  [{ dataSize: 80 }]
);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use futures::StreamExt;
use solana_account_decoder::UiAccountEncoding;
use solana_client::{
    nonblocking::pubsub_client::PubsubClient, rpc_config::RpcProgramAccountsConfig,
    rpc_filter::RpcFilterType,
};
use solana_commitment_config::CommitmentConfig;
use solana_sdk::pubkey::Pubkey;
use std::str::FromStr;

#[tokio::main]
async fn main() -> Result<()> {
    let pubsub_client = PubsubClient::new("wss://api.devnet.solana.com/").await?;

    let program_id = Pubkey::from_str("11111111111111111111111111111111")?;

    let config = RpcProgramAccountsConfig {
        filters: Some(vec![RpcFilterType::DataSize(80)]),
        account_config: solana_client::rpc_config::RpcAccountInfoConfig {
            encoding: Some(UiAccountEncoding::Base64),
            commitment: Some(CommitmentConfig::finalized()),
            data_slice: None,
            min_context_slot: None,
        },
        with_context: None,
        sort_results: None,
    };

    let (mut notifications, unsubscribe) = pubsub_client
        .program_subscribe(&program_id, Some(config))
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

**`0`** · string · required

Pubkey of the `program_id`, as base-58 encoded string

**`1`** · object

Optional configuration object.

**`commitment`** · string · default `finalized`

Values: `processed`, `confirmed`, `finalized`
The commitment describes how finalized a block is at that point in time. See [Configuring State Commitment](https://solana.com/docs/rpc#configuring-state-commitment).

**`filters`** · array

Filter results using up to 4 filter objects. See [Filtering](https://solana.com/docs/rpc#filter-criteria). All filters must match for a given account to be included.

{% hint style="info" %}
The node validates and optimizes the supplied filters before the subscription is created.
{% endhint %}

**`encoding`** · string · default `binary`

Values: `base58`, `base64`, `base64+zstd`, `binary`, `jsonParsed`
Encoding format for account data.

* `base58` is slow.
* `jsonParsed` encoding attempts to use program-specific state parsers to return more human-readable and explicit account state data.
* If `jsonParsed` is requested but no parser is available, the RPC node falls back to `[data, "base64"]`.
* `binary` is a deprecated legacy alias for base58 data returned as a plain string.

**`dataSlice`** · object

Request a slice of each returned account's data.

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

**`withContext`** · bool · default `false`

Values: `true`, `false`
Accepted for compatibility.

{% hint style="info" %}
Unlike [getProgramAccounts](https://solana.com/docs/rpc/http/getprogramaccounts), `programNotification` messages already include `context`. `withContext`, `minContextSlot`, and `sortResults` are accepted for compatibility, but PubSub subscriptions do not currently change behaviour based on those fields.
{% endhint %}

## Response

```json
{
  "jsonrpc": "2.0",
  "result": 24040,
  "id": 1
}
```

## Result

Subscription id. Pass this to [programUnsubscribe](https://solana.com/docs/rpc/websocket/programunsubscribe).

***

_Adapted from the_ [_Solana `programSubscribe` reference_](https://solana.com/docs/rpc/websocket/programsubscribe)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
