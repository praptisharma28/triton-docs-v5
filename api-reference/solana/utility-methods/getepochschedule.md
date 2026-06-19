---
description: Returns the cluster's epoch schedule parameters.
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

# getEpochSchedule

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getEpochSchedule"
}'
```
{% endtab %}

{% tab title="Kit" %}
```typescript
import { createSolanaRpc } from "@solana/kit";

const rpc_url = "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>";
const rpc = createSolanaRpc(rpc_url);

let epochSchedule = await rpc.getEpochSchedule().send();

console.log(epochSchedule);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import { Connection } from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

let epochSchedule = await connection.getEpochSchedule();

console.log(epochSchedule);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use solana_client::nonblocking::rpc_client::RpcClient;
use solana_commitment_config::CommitmentConfig;

#[tokio::main]
async fn main() -> Result<()> {
    let client = RpcClient::new_with_commitment(
        String::from("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>"),
        CommitmentConfig::confirmed(),
    );

    let epoch_schedule = client.get_epoch_schedule().await?;

    println!("{:#?}", epoch_schedule);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "firstNormalEpoch": 8,
    "firstNormalSlot": 8160,
    "leaderScheduleSlotOffset": 8192,
    "slotsPerEpoch": 8192,
    "warmup": true
  },
  "id": 1
}
```

## Result

The result field will be an object with the following fields:

**`firstNormalEpoch`** · u64

The first normal-length epoch after the warmup period. See the [`EpochSchedule`](https://github.com/anza-xyz/solana-sdk/blob/clock%40v2.2.3/epoch-schedule/src/lib.rs#L67-L70) definition in `solana-sdk` for the exact derivation.

**`firstNormalSlot`** · u64

The first slot in the first normal-length epoch. See the [`EpochSchedule`](https://github.com/anza-xyz/solana-sdk/blob/clock%40v2.2.3/epoch-schedule/src/lib.rs#L72-L75) definition in `solana-sdk` for the exact derivation.

**`leaderScheduleSlotOffset`** · u64

The number of slots before beginning of an epoch to calculate a leader schedule for that epoch.

**`slotsPerEpoch`** · u64

The maximum number of slots in each epoch.

**`warmup`** · bool

Whether epochs start short and grow.

***

_Adapted from the_ [_Solana `getEpochSchedule` reference_](https://solana.com/docs/rpc/http/getepochschedule)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
