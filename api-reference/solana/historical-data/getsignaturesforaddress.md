---
description: >-
  Returns confirmed transaction signatures for transactions that reference the
  supplied address in accountKeys, newest first.
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

# getSignaturesForAddress

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getSignaturesForAddress",
  "params": [
    "Vote111111111111111111111111111111111111111",
    {
      "commitment": "finalized",
      "limit": 1
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

let addr = address("Vote111111111111111111111111111111111111111");

let signaturesForConfig = {
  limit: 1
};

let signatures = await rpc
  .getSignaturesForAddress(addr, signaturesForConfig)
  .send();

console.log(signatures);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import {
  Connection,
  PublicKey,
  type SignaturesForAddressOptions
} from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

let signaturesOptions: SignaturesForAddressOptions = {
  limit: 1
};

let address = new PublicKey("Vote111111111111111111111111111111111111111");
let signatures = await connection.getSignaturesForAddress(
  address,
  signaturesOptions
);

console.log(signatures);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use solana_client::{
    nonblocking::rpc_client::RpcClient, rpc_client::GetConfirmedSignaturesForAddress2Config,
};
use solana_commitment_config::CommitmentConfig;
use solana_sdk::pubkey;

#[tokio::main]
async fn main() -> Result<()> {
    let client = RpcClient::new_with_commitment(
        String::from("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>"),
        CommitmentConfig::confirmed(),
    );

    let address = pubkey!("Vote111111111111111111111111111111111111111");

    let signatures_for_config = GetConfirmedSignaturesForAddress2Config {
        before: None,
        until: None,
        limit: Some(1),
        commitment: CommitmentConfig::finalized().into(),
    };

    let signatures = client
        .get_signatures_for_address_with_config(&address, signatures_for_config)
        .await?;

    println!("{:#?}", signatures);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`address`** · string · required

Account address as base-58 encoded string

**`config`** · object

Configuration object containing the following fields:

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

The minimum slot that the request can be evaluated at

**`limit`** · usize · default `1000`

Maximum transaction signatures to return (between 1 and 1,000).

**`before`** · string

Start searching backwards from this transaction signature. If not provided the search starts from the top of the highest max confirmed block.

**`until`** · string

Search until this transaction signature, if found before limit reached

## Response

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "signature": "5h6xBEauJ3PK6SWCZ1PGjBvj8vDdWG3KpwATGy1ARAXFSDwt8GFXM7W5Ncn16wmqokgpiKRLuS83KUxyZyv2sUYv",
      "slot": 114,
      "err": null,
      "memo": null,
      "blockTime": null,
      "confirmationStatus": "finalized"
    }
  ],
  "id": 1
}
```

## Result

An array of transaction signature information objects, ordered from **newest** to **oldest** transaction, containing:

**`signature`** · string

Transaction signature as base-58 encoded string

**`slot`** · u64

The slot that contains the block with the transaction

**`err`** · object | string | null

Error if the transaction failed. `null` if the transaction succeeded.

When `err` is returned as an object, it is a transaction error enum payload rather than a fixed JSON schema:

| Field           | Type                                | Description                                                                                                                                     |
| --------------- | ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| `<variantName>` | `array \| object \| string \| null` | Transaction error variant payload. Most variants serialize as strings. `InstructionError` serializes as `[instructionIndex, instructionError]`. |

**`memo`** · string | null

Memo associated with the transaction, null if no memo is present

**`blockTime`** · i64 | null

Estimated production time, as Unix timestamp (seconds since the Unix epoch) of when transaction was processed. null if not available.

**`confirmationStatus`** · string | null

The transaction's cluster confirmation status; Either `processed`, `confirmed`, or `finalized`. See [Commitment](https://solana.com/docs/rpc/#configuring-state-commitment) for more on optimistic confirmation.

***

_Adapted from the_ [_Solana `getSignaturesForAddress` reference_](https://solana.com/docs/rpc/http/getsignaturesforaddress)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
