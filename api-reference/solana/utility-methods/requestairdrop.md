---
description: Requests a faucet transaction that transfers lamports to the supplied Pubkey.
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

# requestAirdrop

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "requestAirdrop",
  "params": [
    "83astBRguLMdt2h5U1Tpdq5tjFoJ6noeGwaY3mDLVcri",
    1000000000,
    {
      "commitment": "finalized"
    }
  ]
}'
```
{% endtab %}

{% tab title="Kit" %}
```typescript
import { address, createSolanaRpc, lamports } from "@solana/kit";
import { LAMPORTS_PER_SOL } from "@solana/web3.js";

const rpc_url = "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>";
const rpc = createSolanaRpc(rpc_url);

let receiver = address("4kg8oh3jdNtn7j2wcS7TrUua31AgbLzDVkBZgTAe44aF");

let airdropAmt = lamports(BigInt(1 * LAMPORTS_PER_SOL));

let signature = await rpc.requestAirdrop(receiver, airdropAmt).send();

console.log(signature);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import {
  Connection,
  LAMPORTS_PER_SOL,
  PublicKey
} from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

let receiver = new PublicKey("4kg8oh3jdNtn7j2wcS7TrUua31AgbLzDVkBZgTAe44aF");

let airdropAmt = 1 * LAMPORTS_PER_SOL;

let sig = await connection.requestAirdrop(receiver, airdropAmt);

console.log(sig);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use solana_client::nonblocking::rpc_client::RpcClient;
use solana_commitment_config::CommitmentConfig;
use solana_sdk::{native_token::LAMPORTS_PER_SOL, pubkey};

#[tokio::main]
async fn main() -> Result<()> {
    let client = RpcClient::new_with_commitment(
        String::from("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>"),
        CommitmentConfig::confirmed(),
    );

    let receiver = pubkey!("4kg8oh3jdNtn7j2wcS7TrUua31AgbLzDVkBZgTAe44aF");

    let lamports = 1 * LAMPORTS_PER_SOL;

    let transaction_signature = client.request_airdrop(&receiver, lamports).await?;
    loop {
        if client.confirm_transaction(&transaction_signature).await? {
            break;
        }
    }

    println!("{}", transaction_signature);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`pubkey`** · string · required

Pubkey of account to receive lamports, as a base-58 encoded string

**`lamports`** · u64 · required

Amount of lamports to airdrop

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

**`recentBlockhash`** · string

Use this recent blockhash for the faucet transaction instead of the node's current confirmed blockhash

## Response

```json
{
  "jsonrpc": "2.0",
  "result": "4cdd1oX7cfVALfr26tP52BZ6cSzrgnNGtYD7BFhm6FFeZV5sPTnRvg6NRn8yC6DbEikXcrNChBM5vVJnTgKhGhVu",
  "id": 1
}
```

## Result

Transaction Signature of the airdrop, as a base-58 encoded string

***

_Adapted from the_ [_Solana `requestAirdrop` reference_](https://solana.com/docs/rpc/http/requestairdrop)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
