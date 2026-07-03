---
description: >-
  Returns the inflation reward credited to each supplied address for an epoch.
  Depending on the supplied address, this may be a staking reward or a voting
  reward.
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

# getInflationReward

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getInflationReward",
  "params": [
    [
      "6dmNQ5jwLeLk5REvio1JcMshcbvkYMwy26sJ8pbkvStu",
      "BGsqMegLpV6n6Ve146sSX2dTjUMj3M92HnU8BbNRMhF2"
    ],
    {
      "epoch": 800,
      "commitment": "finalized"
    }
  ]
}'
```
{% endtab %}

{% tab title="Kit" %}
```typescript
import { address, createSolanaRpc } from "@solana/kit";

const rpc_url = "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>";
const rpc = createSolanaRpc(rpc_url);

let addresses = [
  address("6dmNQ5jwLeLk5REvio1JcMshcbvkYMwy26sJ8pbkvStu"),
  address("BGsqMegLpV6n6Ve146sSX2dTjUMj3M92HnU8BbNRMhF2")
];

let epoch = BigInt(2);

let inflationReward = await rpc.getInflationReward(addresses, { epoch }).send();

console.log(inflationReward);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import { Connection, PublicKey } from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

let addresses = [
  new PublicKey("6dmNQ5jwLeLk5REvio1JcMshcbvkYMwy26sJ8pbkvStu"),
  new PublicKey("BGsqMegLpV6n6Ve146sSX2dTjUMj3M92HnU8BbNRMhF2")
];

let epoch = 2;

let inflationReward = await connection.getInflationReward(addresses, epoch);

console.log(inflationReward);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use solana_client::nonblocking::rpc_client::RpcClient;
use solana_commitment_config::CommitmentConfig;
use solana_sdk::pubkey;

#[tokio::main]
async fn main() -> Result<()> {
    let client = RpcClient::new_with_commitment(
        String::from("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>"),
        CommitmentConfig::confirmed(),
    );

    let addresses = [
        pubkey!("6dmNQ5jwLeLk5REvio1JcMshcbvkYMwy26sJ8pbkvStu"),
        pubkey!("BGsqMegLpV6n6Ve146sSX2dTjUMj3M92HnU8BbNRMhF2"),
    ];

    let epoch = 2;

    let inflation_reward = client.get_inflation_reward(&addresses, Some(epoch)).await?;

    println!("{:#?}", inflation_reward);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`addresses`** · array · required

An array of addresses to query, as base-58 encoded strings

**`config`** · object · optional

Configuration object containing the following fields:

**`commitment`** · string · default `finalized`

Values: `processed`, `confirmed`, `finalized`

Solana RPC uses the following commitment levels:

| Value       | Description                                                                                                                                                                                                                                                                |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `processed` | Return data from the highest slot this node has processed on the fork it currently considers best. This is the newest view, but it can still change if the cluster switches forks.                                                                                         |
| `confirmed` | Return data from the highest slot that at least two-thirds of active stake has directly voted to confirm. This is more stable than `processed`, but it is still a weaker guarantee than `finalized`.                                                                       |
| `finalized` | Return data from the highest slot that the cluster recognizes as finalized. In practice, this means the slot has reached maximum vote lockout in validators' vote towers and is recognized by at least two-thirds of active stake. This is the strongest commitment level. |

**`epoch`** · u64

An epoch for which the reward occurs. If omitted, the previous epoch will be used

**`minContextSlot`** · number

The minimum slot that the request can be evaluated at

## Response

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "epoch": 2,
      "effectiveSlot": 224,
      "amount": 2500,
      "postBalance": 499999442500,
      "commission": null
    },
    null
  ],
  "id": 1
}
```

## Result

The result field will be a JSON array whose elements are either reward objects or `null` when no reward data is available for the corresponding input address:

**`epoch`** · u64

Epoch for which reward occurred

**`effectiveSlot`** · u64

The slot in which the rewards are effective

**`amount`** · u64

Reward amount in lamports

**`postBalance`** · u64

Post balance of the account in lamports

**`commission`** · u8 | null

Vote account commission when the reward was credited

***

_Adapted from the_ [_Solana `getInflationReward` reference_](https://solana.com/docs/rpc/http/getinflationreward)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
