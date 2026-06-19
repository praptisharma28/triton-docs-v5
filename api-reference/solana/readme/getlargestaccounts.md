---
description: >-
  Returns the 20 largest accounts by lamport balance. Some RPC nodes may serve
  cached results for up to two hours.
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

# getLargestAccounts

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getLargestAccounts",
  "params": [
    {
      "commitment": "finalized",
      "sortResults": true
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

let largestAccounts = await rpc.getLargestAccounts().send();

console.log(largestAccounts);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import {
  Connection,
  type GetLargestAccountsConfig
} from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

let config: GetLargestAccountsConfig = {
  commitment: "finalized",
  filter: "circulating"
};

let largestAccounts = await connection.getLargestAccounts(config);

console.log(largestAccounts);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use solana_client::{
    nonblocking::rpc_client::RpcClient,
    rpc_config::{RpcLargestAccountsConfig, RpcLargestAccountsFilter},
};
use solana_commitment_config::CommitmentConfig;

#[tokio::main]
async fn main() -> Result<()> {
    let client = RpcClient::new_with_commitment(
        String::from("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>"),
        CommitmentConfig::confirmed(),
    );

    let config = RpcLargestAccountsConfig {
        commitment: CommitmentConfig::finalized().into(),
        filter: RpcLargestAccountsFilter::Circulating.into(),
        sort_results: true.into(),
    };
    let largest_accounts = client.get_largest_accounts_with_config(config).await?;

    println!("{:#?}", largest_accounts);

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

**`filter`** · string

Values: `circulating`, `nonCirculating`

Filter results by account type. If omitted, the RPC node includes both circulating and non-circulating accounts.

**`sortResults`** · bool · default `true`

Values: `true`, `false`

Whether to sort the returned accounts by lamport balance before returning them

## Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": { "apiVersion": "3.1.8", "slot": 54 },
    "value": [
      {
        "address": "99P8ZgtJYe1buSK8JXkvpLh8xPsCFuLYhz9hQFNw93WJ",
        "lamports": 999974
      },
      {
        "address": "uPwWLo16MVehpyWqsLkK3Ka8nLowWvAHbBChqv2FZeL",
        "lamports": 42
      }
    ]
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

**`value`** · array

Array of up to 20 account-balance objects with the following fields:

| Field      | Type     | Description                                   |
| ---------- | -------- | --------------------------------------------- |
| `address`  | `string` | Account address, as a base-58 encoded string. |
| `lamports` | `u64`    | Number of lamports in the account.            |

***

_Adapted from the_ [_Solana `getLargestAccounts` reference_](https://solana.com/docs/rpc/http/getlargestaccounts)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
