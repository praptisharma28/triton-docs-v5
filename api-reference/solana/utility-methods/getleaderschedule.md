---
description: >-
  Returns the leader schedule for the epoch that contains the supplied slot, or
  for the current epoch if no slot is supplied.
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

# getLeaderSchedule

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getLeaderSchedule",
  "params": [
    null,
    {
      "commitment": "processed",
      "identity": "dv2eQHeP4RFrJZ6UeiZWoc3XTtmtZCUKxxCApCDcRNV"
    }
  ]
}'
```
{% endtab %}

{% tab title="Kit" %}
```typescript
import { address, createSolanaRpc, type Commitment } from "@solana/kit";

const rpc_url = "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>";
const rpc = createSolanaRpc(rpc_url);

let slotNumber = null;

let identity = address("dv1ZAGvdsz5hHLwWXsVnM94hWf1pjbKVau1QVkaMJ92");
let commitment: Commitment = "finalized";

let leaderSchedule = await rpc
  .getLeaderSchedule(slotNumber, { identity, commitment })
  .send();

console.log(leaderSchedule);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import { Connection } from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

let leaderSchedule = await connection.getLeaderSchedule();

console.log(leaderSchedule);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use solana_client::{nonblocking::rpc_client::RpcClient, rpc_config::RpcLeaderScheduleConfig};
use solana_commitment_config::CommitmentConfig;

#[tokio::main]
async fn main() -> Result<()> {
    let client = RpcClient::new_with_commitment(
        String::from("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>"),
        CommitmentConfig::confirmed(),
    );

    let slot_number = None;
    let config = RpcLeaderScheduleConfig {
        identity: String::from("dv1ZAGvdsz5hHLwWXsVnM94hWf1pjbKVau1QVkaMJ92").into(),
        commitment: CommitmentConfig::finalized().into(),
    };
    let leader_schedule = client
        .get_leader_schedule_with_config(slot_number, config)
        .await?;

    println!("{:#?}", leader_schedule);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`slot`** · u64 | object | null · optional

Either the slot used to select the epoch, `null` to use the current slot at the requested commitment, or a configuration object.

* If this parameter is a number, the RPC returns the leader schedule for the epoch that contains that slot.
* If this parameter is `null` or omitted, the RPC uses the current epoch.
* If this parameter is an object, it has the same shape as `config` and the request omits the slot.

When this parameter is an object, it contains:

| Field        | Type     | Description                                                                      |
| ------------ | -------- | -------------------------------------------------------------------------------- |
| `commitment` | `string` | Commitment level used to select which slot the node reads from for this request. |
| `identity`   | `string` | Validator identity to filter for, as a base-58 encoded string.                   |

**`config`** · object · optional

Configuration object containing the following fields. This parameter is only used when `slot` is a number or `null`.

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

## Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "4Qkev8aNZcqFNSRhQzwyLMFSsi94jHqE8WNVTJzTP99F": [
      0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20,
      21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38,
      39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56,
      57, 58, 59, 60, 61, 62, 63
    ]
  },
  "id": 1
}
```

## Result

Returns `null` if the requested epoch is unavailable. Otherwise returns an object where:

* Keys are validator identities (as base-58 encoded strings)
* Values are arrays of leader slot indices relative to the first slot in the requested epoch

***

_Adapted from the_ [_Solana `getLeaderSchedule` reference_](https://solana.com/docs/rpc/http/getleaderschedule)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
