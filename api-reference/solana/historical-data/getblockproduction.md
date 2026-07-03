---
description: >-
  Returns block-production counts for validator identities in the requested slot
  range. If no range is provided, the RPC node returns counts for the current
  epoch.
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

# getBlockProduction

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getBlockProduction",
  "params": [
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

let blockProduction = await rpc.getBlockProduction().send();

console.log("block production:", blockProduction);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import { Connection } from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

let blockProduction = await connection.getBlockProduction();

console.log("block production:", blockProduction);
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

    let block_production = client.get_block_production().await?;

    println!("Block production: {:#?}", block_production);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`config`** · object · optional

Configuration object

**`commitment`** · string · default `finalized`

Values: `processed`, `confirmed`, `finalized`

Solana RPC uses the following commitment levels:

| Value       | Description                                                                                                                                                                                                                                                                |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `processed` | Return data from the highest slot this node has processed on the fork it currently considers best. This is the newest view, but it can still change if the cluster switches forks.                                                                                         |
| `confirmed` | Return data from the highest slot that at least two-thirds of active stake has directly voted to confirm. This is more stable than `processed`, but it is still a weaker guarantee than `finalized`.                                                                       |
| `finalized` | Return data from the highest slot that the cluster recognizes as finalized. In practice, this means the slot has reached maximum vote lockout in validators' vote towers and is recognized by at least two-thirds of active stake. This is the strongest commitment level. |

**`identity`** · string

Return only entries for this validator identity, as a base-58 encoded string.

**`range`** · object

Slot range to return block production for. If parameter not provided, defaults to current epoch.

| Field       | Type  | Description                                                                                                                                       |
| ----------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| `firstSlot` | `u64` | First slot for which to return block-production information, inclusive.                                                                           |
| `lastSlot`  | `u64` | Last slot for which to return block-production information, inclusive. If omitted, defaults to the current slot at the selected commitment level. |

## Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "apiVersion": "3.1.8",
      "slot": 9887
    },
    "value": {
      "byIdentity": {
        "85iYT5RuzRTDgjyRa3cP8SYhM2j21fj7NhfJ3peu1DPr": [9888, 9886]
      },
      "range": {
        "firstSlot": 0,
        "lastSlot": 9887
      }
    }
  },
  "id": 1
}
```

## Result

RpcResponse object containing:

**`context`** · object

Slot and API version the node used to answer this request.

| Field        | Type     | Description                                                                     |
| ------------ | -------- | ------------------------------------------------------------------------------- |
| `slot`       | `u64`    | Slot at which the node evaluated this request.                                  |
| `apiVersion` | `string` | RPC API version reported by the node. This field may be omitted by older nodes. |

**`value`** · object

Block production object containing:

| Field        | Type     | Description                                                                                                                                                                 |
| ------------ | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `byIdentity` | `object` | Dictionary keyed by validator identity pubkey. Each value is a two-element array containing the number of leader slots assigned and the number of blocks actually produced. |
| `range`      | `object` | Slot range covered by the returned block-production data.                                                                                                                   |

**`byIdentity` entries**

| Field                 | Type             | Description                                                                  |
| --------------------- | ---------------- | ---------------------------------------------------------------------------- |
| `<validatorIdentity>` | `[usize, usize]` | Map entry keyed by a validator identity pubkey, as a base-58 encoded string. |

**`byIdentity` tuple indexes**

| Index | Type    | Description                                                               |
| ----- | ------- | ------------------------------------------------------------------------- |
| `0`   | `usize` | Number of leader slots assigned to that validator in the requested range. |
| `1`   | `usize` | Number of blocks that validator actually produced in that range.          |

**`range` fields**

| Field       | Type  | Description                                                   |
| ----------- | ----- | ------------------------------------------------------------- |
| `firstSlot` | `u64` | First slot in the returned block-production range, inclusive. |
| `lastSlot`  | `u64` | Last slot in the returned block-production range, inclusive.  |

***

_Adapted from the_ [_Solana `getBlockProduction` reference_](https://solana.com/docs/rpc/http/getblockproduction)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
