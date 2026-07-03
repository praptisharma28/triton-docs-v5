---
description: Returns up to limit confirmed block slots, starting at startSlot.
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

# getBlocksWithLimit

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getBlocksWithLimit",
  "params": [
    5,
    3,
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
let limit = 5;
let blocks = await rpc.getBlocksWithLimit(startSlot, limit).send();

console.log("blocks produced:", blocks);
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
    let limit = 5;

    let blocks = client.get_blocks_with_limit(start_slot, limit).await?;

    println!("Blocks produced: {:?}", blocks);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`start slot`** · u64 · required

Start slot

**`limit`** · usize · required

Maximum number of blocks to return. Must not exceed `500000`.

**`config`** · object · optional

Configuration object

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
  "result": [5, 6, 7],
  "id": 1
}
```

## Result

An array of u64 integers listing confirmed blocks starting at `start_slot` for up to `limit` blocks. If `limit` is `0`, the result is an empty array.

***

_Adapted from the_ [_Solana `getBlocksWithLimit` reference_](https://solana.com/docs/rpc/http/getblockswithlimit)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
