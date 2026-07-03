---
description: >-
  Returns recent prioritization-fee samples, optionally filtered to transactions
  that lock a set of writable accounts.
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

# getRecentPrioritizationFees

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getRecentPrioritizationFees",
  "params": [
    ["CxELquR1gPP8wHe33gZ4QxqGB3sZ9RSwsJ2KshVewkFY"]
  ]
}'
```
{% endtab %}

{% tab title="Kit" %}
```typescript
import { address, createSolanaRpc } from "@solana/kit";

const rpc_url = "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>";
const rpc = createSolanaRpc(rpc_url);

let addresses = [address("CxELquR1gPP8wHe33gZ4QxqGB3sZ9RSwsJ2KshVewkFY")];
let prioritizationFees = await rpc
  .getRecentPrioritizationFees(addresses)
  .send();

console.log(prioritizationFees);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import { Connection, PublicKey } from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

let addresses = [new PublicKey("CxELquR1gPP8wHe33gZ4QxqGB3sZ9RSwsJ2KshVewkFY")];

let prioritizationFees = await connection.getRecentPrioritizationFees({
  lockedWritableAccounts: addresses
});

console.log(prioritizationFees);
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

    let addresses = [pubkey!("CxELquR1gPP8wHe33gZ4QxqGB3sZ9RSwsJ2KshVewkFY")];
    let prioritization_fees = client.get_recent_prioritization_fees(&addresses).await?;

    println!("{:#?}", prioritization_fees);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`addresses`** · array · optional

Optional array of account addresses, as base-58 encoded strings. Maximum 128 addresses.

{% hint style="info" %}
If you provide this parameter, the response reflects fees observed for transactions that lock all of the supplied accounts as writable.
{% endhint %}

If you omit `params` entirely, the RPC returns recent prioritization-fee samples without filtering by writable account locks.

## Response

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "slot": 348125,
      "prioritizationFee": 0
    },
    {
      "slot": 348126,
      "prioritizationFee": 1000
    },
    {
      "slot": 348127,
      "prioritizationFee": 500
    },
    {
      "slot": 348128,
      "prioritizationFee": 0
    },
    {
      "slot": 348129,
      "prioritizationFee": 1234
    }
  ],
  "id": 1
}
```

## Result

Array of prioritization-fee samples containing:

**`slot`** · u64

Slot that produced this fee sample.

**`prioritizationFee`** · u64

Prioritization fee, in micro-lamports per compute unit, observed for that slot.

## Triton percentile extension

Solana's `getRecentPrioritizationFees` returns only the _minimum_ fee paid in recent blocks, which is often zero. Triton adds a `percentile` parameter so you can request a realistic market rate.

`percentile` is an integer from 1 to 10000 (0.01% to 100.00%); `5000` is the 50th percentile (median). The response shape is unchanged: `prioritizationFee` reflects the requested percentile instead of the minimum. Omit it for the standard behaviour.

```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getRecentPrioritizationFees",
    "params": [
      ["RNXnAJV1DeBt6Lytjz4wYzvS3d6bhsfidS5Np4ovwZz"],
      { "percentile": 5000 }
    ]
  }'
```

***

_Adapted from the_ [_Solana `getRecentPrioritizationFees` reference_](https://solana.com/docs/rpc/http/getrecentprioritizationfees)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
