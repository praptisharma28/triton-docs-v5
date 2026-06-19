---
description: >-
  Returns vote accounts visible at the requested commitment, partitioned into
  current and delinquent.
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

# getVoteAccounts

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getVoteAccounts",
  "params": [
    {
      "commitment": "finalized",
      "votePubkey": "i7NyKBMJCA9bLM2nsGyAGCKHECuR2L5eh4GqFciuwNT"
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

let votePubkey = address("5ZWgXcyqrrNpQHCme5SdC5hCeYb2o3fEJhF7Gok3bTVN");

let voteAccounts = await rpc
  .getVoteAccounts({
    votePubkey
  })
  .send();

console.log(voteAccounts);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import { Connection, PublicKey } from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

let voteAccounts = await connection.getVoteAccounts();

console.log(voteAccounts);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use solana_client::{nonblocking::rpc_client::RpcClient, rpc_config::RpcGetVoteAccountsConfig};
use solana_commitment_config::CommitmentConfig;

#[tokio::main]
async fn main() -> Result<()> {
    let client = RpcClient::new_with_commitment(
        String::from("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>"),
        CommitmentConfig::confirmed(),
    );

    let vote_pubkey = String::from("5ZWgXcyqrrNpQHCme5SdC5hCeYb2o3fEJhF7Gok3bTVN");

    let config = RpcGetVoteAccountsConfig {
        vote_pubkey: Some(vote_pubkey),
        commitment: CommitmentConfig::finalized().into(),
        keep_unstaked_delinquents: None,
        delinquent_slot_distance: None,
    };

    let vote_accounts = client.get_vote_accounts_with_config(config).await?;

    println!("{:#?}", vote_accounts);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

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

**`votePubkey`** · string

Return only entries for this vote account address, as a base-58 encoded string.

**`keepUnstakedDelinquents`** · bool · default `false`

Values: `true`, `false`

Keep delinquent vote accounts even if their activated stake is zero.

**`delinquentSlotDistance`** · u64 · default `128`

Number of slots a validator may trail the selected slot before the RPC node classifies it as delinquent. **Note:** ecosystem tools usually leave this unset to preserve consistent behavior.

## Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "current": [
      {
        "activatedStake": 38263229364446900,
        "commission": 95,
        "epochCredits": [
          [902, 1383125544, 1376213656],
          [903, 1390037304, 1383125544],
          [904, 1396949288, 1390037304],
          [905, 1403861272, 1396949288],
          [906, 1406766600, 1403861272]
        ],
        "epochVoteAccount": true,
        "lastVote": 391573587,
        "nodePubkey": "dv2eQHeP4RFrJZ6UeiZWoc3XTtmtZCUKxxCApCDcRNV",
        "rootSlot": 391573556,
        "votePubkey": "i7NyKBMJCA9bLM2nsGyAGCKHECuR2L5eh4GqFciuwNT"
      }
    ],
    "delinquent": []
  },
  "id": 1
}
```

## Result

Vote-account status object containing `current` and `delinquent` arrays of vote account records.

**`current`** · array

Vote accounts the RPC node classifies as current at the requested commitment.

**`delinquent`** · array

Vote accounts the RPC node classifies as delinquent at the requested commitment.

Each element of `current` and `delinquent` is a vote-account object containing:

| Field              | Type     | Description                                                                                                  |
| ------------------ | -------- | ------------------------------------------------------------------------------------------------------------ |
| `activatedStake`   | `u64`    | Stake, in lamports, delegated to this vote account and active in the current epoch.                          |
| `commission`       | `u8`     | Commission rate used for rewards payout, expressed as an 8-bit fraction.                                     |
| `epochCredits`     | `array`  | Recent history of earned credits for up to five epochs. Each element is `[epoch, credits, previousCredits]`. |
| `epochVoteAccount` | `bool`   | Whether this vote account is staked for the current epoch.                                                   |
| `lastVote`         | `u64`    | Most recent slot this vote account voted on. Returns `0` if no votes exist.                                  |
| `nodePubkey`       | `string` | Validator identity pubkey, as a base-58 encoded string.                                                      |
| `rootSlot`         | `u64`    | Current root slot for this vote account. Returns `0` if no root slot exists.                                 |
| `votePubkey`       | `string` | Vote account pubkey, as a base-58 encoded string.                                                            |

***

_Adapted from the_ [_Solana `getVoteAccounts` reference_](https://solana.com/docs/rpc/http/getvoteaccounts)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
