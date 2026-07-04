---
description: Returns the latest blockhash and its last valid block height.
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

# getLatestBlockhash

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getLatestBlockhash",
  "params": [
    {
      "commitment": "processed"
    }
  ]
}'
```
{% endtab %}

{% tab title="Kit" %}
```typescript
import { createSolanaRpc, type Commitment } from "@solana/kit";

const rpc_url = "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>";
const rpc = createSolanaRpc(rpc_url);

let commitment: Commitment = "processed";
let latestBlockhash = await rpc.getLatestBlockhash({ commitment }).send();

console.log(latestBlockhash);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import { Connection, type Commitment } from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

let commitment: Commitment = "processed";
let latestBlockhash = await connection.getLatestBlockhash(commitment);

console.log(latestBlockhash);
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

    let commitment = CommitmentConfig::processed();
    let latest_blockhash = client
        .get_latest_blockhash_with_commitment(commitment)
        .await?;

    println!("{:#?}", latest_blockhash);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`config`** · object

Configuration object containing the following fields:

**`commitment`** · string · default `finalized`

Values: `processed`, `confirmed`, `finalized`

Solana RPC uses the following commitment levels:

| Value       | Description                                                                                                                                                                                                                                                                |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `processed` | Return data from the highest slot this node has processed on the fork it currently considers best. This is the newest view, but it can still change if the cluster switches forks.                                                                                         |
| `confirmed` | Return data from the highest slot that at least two-thirds of active stake has directly voted to confirm. This is more stable than `processed`, but it is still a weaker guarantee than `finalized`.                                                                       |
| `finalized` | Return data from the highest slot that the cluster recognizes as finalized. In practice, this means the slot has reached maximum vote lockout in validators' vote towers and is recognized by at least two-thirds of active stake. This is the strongest commitment level. |

**`minContextSlot`** · number

The minimum slot that the request can be evaluated at

## Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "apiVersion": "3.1.8",
      "slot": 2792
    },
    "value": {
      "blockhash": "EkSnNWid2cvwEVnVx9aBqawnmiCNiDgp3gUdkDPTKN1N",
      "lastValidBlockHeight": 3090
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

Latest blockhash response object with the following fields:

| Field                  | Type     | Description                                                                                                             |
| ---------------------- | -------- | ----------------------------------------------------------------------------------------------------------------------- |
| `blockhash`            | `string` | Latest blockhash, as a base-58 encoded string.                                                                          |
| `lastValidBlockHeight` | `u64`    | Last [block height](https://solana.com/docs/references/terminology#block-height) at which this blockhash remains valid. |

***

_Adapted from the_ [_Solana `getLatestBlockhash` reference_](https://solana.com/docs/rpc/http/getlatestblockhash)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
