---
description: >-
  Returns the current status for each supplied transaction signature. Each
  signature must be a txid, the first signature of a transaction.
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

# getSignatureStatuses

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getSignatureStatuses",
  "params": [
    [
      "4cdd1oX7cfVALfr26tP52BZ6cSzrgnNGtYD7BFhm6FFeZV5sPTnRvg6NRn8yC6DbEikXcrNChBM5vVJnTgKhGhVu"
    ],
    {
      "searchTransactionHistory": true
    }
  ]
}'
```
{% endtab %}

{% tab title="Kit" %}
```typescript
import { createSolanaRpc, type Signature } from "@solana/kit";

const rpc_url = "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>";
const rpc = createSolanaRpc(rpc_url);

let signatures = [
  "4cdd1oX7cfVALfr26tP52BZ6cSzrgnNGtYD7BFhm6FFeZV5sPTnRvg6NRn8yC6DbEikXcrNChBM5vVJnTgKhGhVu" as unknown as Signature
];

let config = {
  searchTransactionHistory: true
};

let signatureStatus = await rpc.getSignatureStatuses(signatures, config).send();

console.log(signatureStatus);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import {
  Connection,
  type SignatureStatusConfig
} from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

let signatures = [
  "4cdd1oX7cfVALfr26tP52BZ6cSzrgnNGtYD7BFhm6FFeZV5sPTnRvg6NRn8yC6DbEikXcrNChBM5vVJnTgKhGhVu"
];

let config: SignatureStatusConfig = {
  searchTransactionHistory: true
};

let signatureStatus = await connection.getSignatureStatuses(signatures, config);
console.log(signatureStatus);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use solana_client::nonblocking::rpc_client::RpcClient;
use solana_commitment_config::CommitmentConfig;
use solana_sdk::signature::Signature;
use std::str::FromStr;

#[tokio::main]
async fn main() -> Result<()> {
    let client = RpcClient::new_with_commitment(
        String::from("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>"),
        CommitmentConfig::confirmed(),
    );

    let signatures_str = [
        "4cdd1oX7cfVALfr26tP52BZ6cSzrgnNGtYD7BFhm6FFeZV5sPTnRvg6NRn8yC6DbEikXcrNChBM5vVJnTgKhGhVu",
    ];
    let signatures = signatures_str.map(|sig| Signature::from_str(sig).unwrap());

    let signature_status = client
        .get_signature_statuses_with_history(&signatures)
        .await?;

    println!("{:#?}", signature_status);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`signatures`** · array · required

An array of transaction signatures to confirm, as base-58 encoded strings (up to a maximum of 256)

**`config`** · object

Configuration object containing the following fields:

**`searchTransactionHistory`** · bool · default `false`

Values: `true`, `false`

if `true` - a Solana node will search its ledger cache for any signatures not found in the recent status cache

## Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": { "apiVersion": "3.1.8", "slot": 82 },
    "value": [
      {
        "slot": 48,
        "confirmations": null,
        "err": null,
        "status": {
          "Ok": null
        },
        "confirmationStatus": "finalized"
      },
      null
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

Array consisting of either `null` or a transaction-status object containing the following fields:

| Field                | Type                       | Description                                                                                                            |
| -------------------- | -------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| `slot`               | `u64`                      | Slot in which the transaction was processed.                                                                           |
| `confirmations`      | `usize \| null`            | Number of blocks since confirmation. `null` means the transaction is rooted and finalized by a supermajority of stake. |
| `err`                | `object \| string \| null` | Error if the transaction failed. `null` if the transaction succeeded.                                                  |
| `status`             | `object`                   | Deprecated legacy status object that mirrors `err`.                                                                    |
| `confirmationStatus` | `string \| null`           | Cluster confirmation status. When present, the value is `processed`, `confirmed`, or `finalized`.                      |

When `err` is returned as an object, it is a transaction error enum payload rather than a fixed JSON schema:

| Field           | Type                                | Description                                                                                                                                     |
| --------------- | ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| `<variantName>` | `array \| object \| string \| null` | Transaction error variant payload. Most variants serialize as strings. `InstructionError` serializes as `[instructionIndex, instructionError]`. |

**`status` fields**

**DEPRECATED** Legacy transaction status object. Exactly one of these fields is present:

| Field | Type               | Description                                                       |
| ----- | ------------------ | ----------------------------------------------------------------- |
| `Ok`  | `null`             | Transaction succeeded.                                            |
| `Err` | `object \| string` | Transaction failed. Payload uses the same serialization as `err`. |

***

_Adapted from the_ [_Solana `getSignatureStatuses` reference_](https://solana.com/docs/rpc/http/getsignaturestatuses)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
