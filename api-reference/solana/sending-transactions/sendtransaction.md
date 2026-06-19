---
description: >-
  Submits a signed transaction to the cluster and returns its first signature if
  the RPC accepts it.
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

# sendTransaction

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sendTransaction",
  "params": [
    "AX9ymHgILTd01GtdBsbQZ9by5D4z4UWh+1Tsh6LDuPHuZk/Z8LrIwoKZRp3le2RTb5lywL1xcJGQ8u3M3U0tyAgBAAEDXCL+JarHftfP5QgsRxpEsr+/YiRWQeCz7PI3jfzeppEvUc61u+TNHri9ChQdLN2evWGx+1O3MwbPFKV5rIqlxAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAvOdVbvGn3R+uBkugNCq8C/Texc7ILBoYjbn4aVQV0OgBAgIAAQwCAAAAQEIPAAAAAAA=",
    {
      "encoding": "base64",
      "skipPreflight": true
    }
  ]
}'
```
{% endtab %}

{% tab title="Kit" %}
```typescript
import {
  createSolanaRpc,
  type Base64EncodedWireTransaction
} from "@solana/kit";

const rpc_url = "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>";
const rpc = createSolanaRpc(rpc_url);

const base64Tx =
  "AbC/XNkPUUZ7/51SaG1wbG0ojrWHIGzVL73M8hRnDr73RkBAZc0ZnikluvcCeprAmqHDJrcPxPUbvEJMVBIiVQeAAQABAzfDSQC/GjcggrLsDpYz7jAlT+Gca846HqtFb8UQMM9cCWPIi4AX32PV8HrY7/1WgoRc3IATttceZsUMeQ1qx7UAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIgsVWEgMTiOYp63gTtuYGw+izfm6wKQdivpiXQBpNnYAQICAAEMAgAAAEBCDwAAAAAAAA==";

const signature = await rpc
  .sendTransaction(base64Tx as Base64EncodedWireTransaction, {
    encoding: "base64"
  })
  .send();

console.log(signature);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import {
  Connection,
  VersionedTransaction
} from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

const base64Tx =
  "AbuRLtc5C9bZtAUT4F4Y2H5SRRUK1HwOFZOK3V4qm/78MDJt+M2de/RCCaI3iTyodDepmrkUWbss0XRHS0lk5AOAAQABAzfDSQC/GjcggrLsDpYz7jAlT+Gca846HqtFb8UQMM9cCWPIi4AX32PV8HrY7/1WgoRc3IATttceZsUMeQ1qx7UAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD2dTRgcJmzcoGH1R3c2WqtHah2H19KvbC1p6BxLDqfoAQICAAEMAgAAAADKmjsAAAAAAA==";

let tx = VersionedTransaction.deserialize(Buffer.from(base64Tx, "base64"));

let sig = await connection.sendTransaction(tx);

console.log(sig);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use base64::{Engine as _, engine::general_purpose};
use solana_client::{nonblocking::rpc_client::RpcClient, rpc_config::RpcSendTransactionConfig};
use solana_commitment_config::{CommitmentConfig, CommitmentLevel};
use solana_sdk::transaction::VersionedTransaction;
use solana_transaction_status_client_types::UiTransactionEncoding;

#[tokio::main]
async fn main() -> Result<()> {
    let client = RpcClient::new_with_commitment(
        String::from("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>"),
        CommitmentConfig::confirmed(),
    );

    let b64_tx = "AbuRLtc5C9bZtAUT4F4Y2H5SRRUK1HwOFZOK3V4q...mjsAAAAAAA==";
    let tx_bytes = general_purpose::STANDARD.decode(b64_tx).unwrap();
    let tx: VersionedTransaction = bincode::deserialize(&tx_bytes).unwrap();

    let config = RpcSendTransactionConfig {
        skip_preflight: true,
        preflight_commitment: CommitmentLevel::Finalized.into(),
        encoding: UiTransactionEncoding::Base64.into(),
        max_retries: None,
        min_context_slot: None,
    };

    match client.send_transaction_with_config(&tx, config).await {
        Ok(signature) => println!("Transaction Signature: {}", signature),
        Err(err) => eprintln!("Error transferring tokens: {}", err),
    }

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`transaction`** · string · required

Fully-signed Transaction, as encoded string.

**`config`** · object · optional

Configuration object containing the following fields:

**`encoding`** · string · default `base58`

Values: `binary`, `base58`, `base64`

Encoding used for the transaction data.

| Encoding | Notes                                                                          |
| -------- | ------------------------------------------------------------------------------ |
| `base64` | Recommended                                                                    |
| `base58` | Slow                                                                           |
| `binary` | ⚠️ Deprecated. Legacy alias for `base58`, retained for backwards compatibility |

**`skipPreflight`** · bool · default `false`

Values: `true`, `false`

When `true`, skip the preflight transaction checks. Default: `false`.

**`preflightCommitment`** · string · default `finalized`

Values: `processed`, `confirmed`, `finalized`

Commitment level to use for preflight when `skipPreflight` is `false`. Default `finalized`. When `skipPreflight` is `true`, this field is ignored and the RPC node uses `processed` internally.

Solana RPC uses the following commitment levels:

| Value       | Description                                                                                                                                                                                                                                                                |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `processed` | Return data from the highest slot this node has processed on the fork it currently considers best. This is the newest view, but it can still change if the cluster switches forks.                                                                                         |
| `confirmed` | Return data from the highest slot that at least two-thirds of active stake has directly voted to confirm. This is more stable than `processed`, but it is still a weaker guarantee than `finalized`.                                                                       |
| `finalized` | Return data from the highest slot that the cluster recognizes as finalized. In practice, this means the slot has reached maximum vote lockout in validators' vote towers and is recognized by at least two-thirds of active stake. This is the strongest commitment level. |

**`maxRetries`** · usize

Maximum number of times for the RPC node to retry sending the transaction to the leader. If this parameter not provided, the RPC node will retry the transaction until it is finalized or until the blockhash expires.

**`minContextSlot`** · number · optional

Set the minimum slot at which to perform preflight transaction checks

## Response

```json
{
  "jsonrpc": "2.0",
  "result": "2id3YC2jK9G5Wo2phDx4gJVAew8DcY5NAojnVuao8rkxwPYPe8cSwE5GzhEgJA2y8fVjDEo6iR6ykBvDxrTQrtpb",
  "id": 1
}
```

## Result

First Transaction Signature embedded in the transaction, as base-58 encoded string ([transaction id](https://solana.com/docs/references/terminology#transaction-id))

***

_Adapted from the_ [_Solana `sendTransaction` reference_](https://solana.com/docs/rpc/http/sendtransaction)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
