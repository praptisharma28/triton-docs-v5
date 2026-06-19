---
description: Returns the token balance for an SPL Token account.
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

# getTokenAccountBalance

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getTokenAccountBalance",
  "params": [
    "48gpnn8nsmkvkgso7462Z1nFhUrprGQ71u1YLBPzizbY",
    {
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

let tokenAddress = address("48gpnn8nsmkvkgso7462Z1nFhUrprGQ71u1YLBPzizbY");

let tokenBalance = await rpc.getTokenAccountBalance(tokenAddress).send();

console.log(tokenBalance);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import { Connection, PublicKey } from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

let tokenAddress = new PublicKey(
  "48gpnn8nsmkvkgso7462Z1nFhUrprGQ71u1YLBPzizbY"
);

let tokenBalance = await connection.getTokenAccountBalance(tokenAddress);

console.log(tokenBalance);
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

    let token_address = pubkey!("48gpnn8nsmkvkgso7462Z1nFhUrprGQ71u1YLBPzizbY");
    let token_acc_bal = client.get_token_account_balance(&token_address).await?;

    println!("{:#?}", token_acc_bal);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`pubkey`** · string · required

Pubkey of Token account to query, as base-58 encoded string

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

## Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": { "apiVersion": "3.1.8", "slot": 1114 },
    "value": {
      "amount": "9864",
      "decimals": 2,
      "uiAmount": 98.64,
      "uiAmountString": "98.64"
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

Token amount object with the following fields:

This uses the same amount fields documented for `uiTokenAmount` in [Token Balances](https://solana.com/docs/rpc/json-structures#token-balances).

| Field            | Type             | Description                                                                                       |
| ---------------- | ---------------- | ------------------------------------------------------------------------------------------------- |
| `amount`         | `string`         | Raw token balance, as a base-10 integer string with no decimal point.                             |
| `decimals`       | `u8`             | Number of decimal places configured on the mint.                                                  |
| `uiAmount`       | `number \| null` | Decimal-scaled token balance as a floating-point number. Deprecated in favor of `uiAmountString`. |
| `uiAmountString` | `string`         | Decimal-scaled token balance as a string.                                                         |

***

_Adapted from the_ [_Solana `getTokenAccountBalance` reference_](https://solana.com/docs/rpc/http/gettokenaccountbalance)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
