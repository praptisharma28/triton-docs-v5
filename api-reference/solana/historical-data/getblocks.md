---
description: >-
  Returns confirmed block slots in the inclusive range starting at startSlot and
  ending at endSlot, if provided.
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

# getBlocks

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getBlocks",
  "params": [
    377268280,
    377268285,
    {
      "commitment": "finalized"
    }
  ]
}'
```
{% endtab %}

{% tab title="Kit" %}
```typescript
import { createSolanaRpc } from "@solana/kit";

const rpc_url = "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>";
const rpc = createSolanaRpc(rpc_url);

let startSlot = BigInt(377268280);
let endSlot = BigInt(377268285);
let blocks = await rpc.getBlocks(startSlot, endSlot).send();

console.log("Blocks produced:", blocks);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import { Connection } from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

let startSlot = 377268280;
let endSlot = 377268285;
let blocks = await connection.getBlocks(startSlot, endSlot);

console.log("Blocks produced:", blocks);
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

    let start_slot = 377268280;
    let end_slot = 377268285;
    let blocks = client.get_blocks(start_slot, Some(end_slot)).await?;

    println!("Blocks produced: {:#?}", blocks);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`start slot`** · u64 · required

Start slot

**`end slot`** · u64 | object · optional

Inclusive end slot. For backwards compatibility, this parameter position may also be used for a configuration object.

* If this parameter is a number, it is the inclusive end slot and must be no more than 500,000 slots higher than `start slot`.
* If this parameter is an object, it has the same shape as `config` and the request omits `end slot`.

When this parameter is an object, it contains:

| Field            | Type     | Description                                                                                                    |
| ---------------- | -------- | -------------------------------------------------------------------------------------------------------------- |
| `commitment`     | `string` | Commitment level used to select which slot the node reads from for this request. `processed` is not supported. |
| `minContextSlot` | `number` | Minimum slot at which the request may be evaluated.                                                            |

**`config`** · object · optional

Configuration object. This parameter is only used when `end slot` is provided as a number.

**`commitment`** · string · default `finalized`

Values: `confirmed`, `finalized`

Solana RPC uses the following commitment levels:

| Value       | Description                                                                                                                                                                                                                                                                |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `processed` | Return data from the highest slot this node has processed on the fork it currently considers best. This is the newest view, but it can still change if the cluster switches forks.                                                                                         |
| `confirmed` | Return data from the highest slot that at least two-thirds of active stake has directly voted to confirm. This is more stable than `processed`, but it is still a weaker guarantee than `finalized`.                                                                       |
| `finalized` | Return data from the highest slot that the cluster recognizes as finalized. In practice, this means the slot has reached maximum vote lockout in validators' vote towers and is recognized by at least two-thirds of active stake. This is the strongest commitment level. |

This method does not accept `processed`.

**`minContextSlot`** · number

The minimum slot that the request can be evaluated at.

## Response

```json
{
  "jsonrpc": "2.0",
  "result": [5, 6, 7, 8, 9, 10],
  "id": 1
}
```

## Result

An array of u64 integers listing confirmed blocks between `start_slot` and either `end_slot` - if provided, or the highest slot available at the requested commitment, inclusive. Max range allowed is 500,000 slots. If `end_slot` is lower than `start_slot`, the result is an empty array.

***

_Adapted from the_ [_Solana `getBlocks` reference_](https://solana.com/docs/rpc/http/getblocks)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
