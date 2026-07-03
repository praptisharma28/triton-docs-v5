---
description: >-
  Returns a confirmed transaction by signature, or null if it is not found at
  the requested commitment.
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

# getTransaction

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getTransaction",
  "params": [
    "4ReKprwf3WdLHRrzp4ctPWNBsQDPL3VZz3zMmoZfcGJMJCHh5Vq937mPdyxhCbw54wNnA6hZ7KfNpQdpt13yY7A9",
    {
      "commitment": "confirmed",
      "maxSupportedTransactionVersion": 0,
      "encoding": "json"
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

let signature =
  "5zSQuTcWsPy2cVAshBXWuJJXLwMD1GbgMpz3iq4xgwiV1s6mxYRbYb7qBiRGZd1xvDcYhQQRBKoNcnW8eKtcyZWg";

let transaction = await rpc.getTransaction(signature as Signature).send();

console.log(transaction);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import {
  Connection,
  PublicKey,
  type GetVersionedTransactionConfig
} from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

let signature =
  "5zSQuTcWsPy2cVAshBXWuJJXLwMD1GbgMpz3iq4xgwiV1s6mxYRbYb7qBiRGZd1xvDcYhQQRBKoNcnW8eKtcyZWg";

let config: GetVersionedTransactionConfig = {
  commitment: "finalized",
  maxSupportedTransactionVersion: 0
};

let transaction = await connection.getTransaction(signature, config);

console.log(transaction);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use solana_client::{nonblocking::rpc_client::RpcClient, rpc_config::RpcTransactionConfig};
use solana_commitment_config::CommitmentConfig;
use solana_sdk::signature::Signature;
use solana_transaction_status_client_types::UiTransactionEncoding;
use std::str::FromStr;

#[tokio::main]
async fn main() -> Result<()> {
    let client = RpcClient::new_with_commitment(
        String::from("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>"),
        CommitmentConfig::confirmed(),
    );

    let tx_sig = Signature::from_str(
        "5zSQuTcWsPy2cVAshBXWuJJXLwMD1GbgMpz3iq4xgwiV1s6mxYRbYb7qBiRGZd1xvDcYhQQRBKoNcnW8eKtcyZWg",
    )?;

    let config = RpcTransactionConfig {
        commitment: CommitmentConfig::finalized().into(),
        encoding: UiTransactionEncoding::Json.into(),
        max_supported_transaction_version: Some(0),
    };

    let transaction = client.get_transaction_with_config(&tx_sig, config).await?;

    println!("{:#?}", transaction);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`signature`** · string · required

Transaction signature, as base-58 encoded string

**`config`** · string | object · optional

Either a configuration object or, for backwards compatibility, a bare encoding string. Prefer the object form.

{% hint style="info" %}
Passing a bare encoding string as the second positional parameter is deprecated. Use the configuration object instead.
{% endhint %}

When this parameter is an object, it may contain the following fields:

**`commitment`** · string · default `finalized`

Values: `confirmed`, `finalized`

Solana RPC uses the following commitment levels:

| Value       | Description                                                                                                                                                                                                                                                                |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `processed` | Return data from the highest slot this node has processed on the fork it currently considers best. This is the newest view, but it can still change if the cluster switches forks.                                                                                         |
| `confirmed` | Return data from the highest slot that at least two-thirds of active stake has directly voted to confirm. This is more stable than `processed`, but it is still a weaker guarantee than `finalized`.                                                                       |
| `finalized` | Return data from the highest slot that the cluster recognizes as finalized. In practice, this means the slot has reached maximum vote lockout in validators' vote towers and is recognized by at least two-thirds of active stake. This is the strongest commitment level. |

This method does not accept `processed`.

**`maxSupportedTransactionVersion`** · number

Values: `0`

Currently, the only valid value for this parameter is `0`. Setting it to `0` allows you to fetch all transactions, including both Versioned and legacy transactions.

This parameter determines the maximum transaction version that will be returned in the response. If you request a transaction with a higher version than this value, an error will be returned. If you omit this parameter, only legacy transactions will be returned; any versioned transaction will result in an error.

**`encoding`** · string · default `json`

Values: `binary`, `base58`, `base64`, `json`, `jsonParsed`

Encoding for the returned transaction.

| Encoding     | Transaction format | Notes                                                                                                                                                              |
| ------------ | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `json`       | JSON object        | Default. Returns parsed transaction with `message` and `signatures`                                                                                                |
| `jsonParsed` | JSON object        | Uses program-specific parsers for `transaction.message.instructions`. Falls back to standard JSON fields (`accounts`, `data`, `programIdIndex`) if no parser found |
| `base64`     | `[data, "base64"]` | Base64 encoded binary transaction                                                                                                                                  |
| `base58`     | `[data, "base58"]` | Slow                                                                                                                                                               |
| `binary`     | `[data, "base58"]` | ⚠️ Deprecated. Legacy alias for `base58`, retained for backwards compatibility                                                                                     |

For `jsonParsed`, this page documents the stable outer shapes returned for parsed account keys and for parsed or partially decoded instructions. The nested `parsed` payload remains program-specific.

If you use the deprecated shorthand form for `config`, pass one of these encoding strings directly.

For the shared JSON structures used by `json` and `jsonParsed` responses, see [Transactions](https://solana.com/docs/rpc/json-structures#transactions), [Parsed Accounts](https://solana.com/docs/rpc/json-structures#parsed-accounts), [Inner Instructions](https://solana.com/docs/rpc/json-structures#inner-instructions), and [Address Table Lookups](https://solana.com/docs/rpc/json-structures#address-table-lookups).

## Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "blockTime": 1763747336,
    "meta": {
      "computeUnitsConsumed": 150,
      "costUnits": 1481,
      "err": null,
      "fee": 5000,
      "innerInstructions": [],
      "loadedAddresses": {
        "readonly": [],
        "writable": []
      },
      "logMessages": [
        "Program 11111111111111111111111111111111 invoke [1]",
        "Program 11111111111111111111111111111111 success"
      ],
      "postBalances": [13086425097431981, 1000000000, 1],
      "postTokenBalances": [],
      "preBalances": [13086426097436981, 0, 1],
      "preTokenBalances": [],
      "rewards": [],
      "status": {
        "Ok": null
      }
    },
    "slot": 423108383,
    "transaction": {
      "message": {
        "accountKeys": [
          "9B5XszUGdMaxCZ7uSQhPzdks5ZQSmWxrmzCSvtJ6Ns6g",
          "D1UPrG5kHPhEvwAoYpFymQkHsNNzx8jWooeAvKiPfgFQ",
          "11111111111111111111111111111111"
        ],
        "header": {
          "numReadonlySignedAccounts": 0,
          "numReadonlyUnsignedAccounts": 1,
          "numRequiredSignatures": 1
        },
        "instructions": [
          {
            "accounts": [0, 1],
            "data": "3Bxs3zzLZLuLQEYX",
            "programIdIndex": 2,
            "stackHeight": 1
          }
        ],
        "recentBlockhash": "TT2s5saKFUnuF8HkdtG6qdkgSSGcFZsuoq8XeuG7iPV"
      },
      "signatures": [
        "4ReKprwf3WdLHRrzp4ctPWNBsQDPL3VZz3zMmoZfcGJMJCHh5Vq937mPdyxhCbw54wNnA6hZ7KfNpQdpt13yY7A9"
      ]
    },
    "version": "legacy"
  },
  "id": 1
}
```

## Result

Returns `null` if transaction is not found or not confirmed, otherwise returns an object containing:

This top-level response uses the shared [Confirmed Transaction Responses](https://solana.com/docs/rpc/json-structures#confirmed-transaction-responses) structure.

**`blockTime`** · i64 | null

Estimated production time, as Unix timestamp (seconds since the Unix epoch) of when the transaction was processed. `null` if not available.

**`meta`** · object | null

Transaction status metadata object, or `null` if the transaction metadata is not available.

For the shared field reference, see [Transaction Status Metadata](https://solana.com/docs/rpc/json-structures#transaction-status-metadata).

Source Type Definition: [UiTransactionStatusMeta](https://github.com/anza-xyz/agave/blob/v3.1.8/transaction-status-client-types/src/lib.rs#L332)

When present, this object may contain:

| Field                  | Type                       | Description                                                                                                  |
| ---------------------- | -------------------------- | ------------------------------------------------------------------------------------------------------------ |
| `err`                  | `object \| string \| null` | Transaction error. `null` indicates success.                                                                 |
| `status`               | `object`                   | Deprecated status object that mirrors `err`. It is `"Ok": <null>` for success or `"Err": <ERR>` for failure. |
| `fee`                  | `u64`                      | Fee charged for the transaction, in lamports.                                                                |
| `preBalances`          | `array`                    | Lamport balances before execution, indexed to `transaction.message.accountKeys`.                             |
| `postBalances`         | `array`                    | Lamport balances after execution, indexed to `transaction.message.accountKeys`.                              |
| `innerInstructions`    | `array \| null`            | CPI instructions recorded during execution. This field may be `null` or omitted when unavailable.            |
| `logMessages`          | `array \| null`            | Program log output captured during execution. This field may be `null` or omitted when unavailable.          |
| `preTokenBalances`     | `array \| null`            | Token balances before execution. Each element uses the token-balance schema documented below.                |
| `postTokenBalances`    | `array \| null`            | Token balances after execution. Each element uses the token-balance schema documented below.                 |
| `rewards`              | `array \| null`            | Rewards applied while processing the transaction. Each element uses the reward schema documented below.      |
| `loadedAddresses`      | `object`                   | Addresses loaded from address lookup tables, when present.                                                   |
| `returnData`           | `object`                   | Most recent program return data, when present.                                                               |
| `computeUnitsConsumed` | `u64`                      | Total compute units consumed, when reported by the validator.                                                |
| `costUnits`            | `u64`                      | Total cost-model units charged, when reported by the validator.                                              |

**`err` object form**

| Field           | Type                                | Description                                                                                                                                     |
| --------------- | ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| `<variantName>` | `array \| object \| string \| null` | Transaction error variant payload. Most variants serialize as strings. `InstructionError` serializes as `[instructionIndex, instructionError]`. |

**`status` fields**

| Field | Type               | Description                                                       |
| ----- | ------------------ | ----------------------------------------------------------------- |
| `Ok`  | `null`             | Deprecated success marker.                                        |
| `Err` | `object \| string` | Deprecated failure payload. Uses the same serialization as `err`. |

**`loadedAddresses` fields**

| Field      | Type    | Description                                                            |
| ---------- | ------- | ---------------------------------------------------------------------- |
| `writable` | `array` | Ordered list of writable loaded addresses, as base-58 encoded strings. |
| `readonly` | `array` | Ordered list of readonly loaded addresses, as base-58 encoded strings. |

**`returnData` fields**

| Field       | Type                 | Description                                                                              |
| ----------- | -------------------- | ---------------------------------------------------------------------------------------- |
| `programId` | `string`             | Program that generated the return data, as a base-58 encoded Pubkey.                     |
| `data`      | `[string, encoding]` | Return data payload and encoding. This field uses base-64 data with `"base64"` encoding. |

**`token balance` fields**

| Field           | Type     | Description                                                                                           |
| --------------- | -------- | ----------------------------------------------------------------------------------------------------- |
| `accountIndex`  | `u8`     | Index into `transaction.message.accountKeys` for the token account whose balance is reported.         |
| `mint`          | `string` | Token mint pubkey, as a base-58 encoded string.                                                       |
| `uiTokenAmount` | `object` | Token amount object with raw and decimal-scaled representations.                                      |
| `owner`         | `string` | Token account owner pubkey, when the validator recorded it. This field may be omitted.                |
| `programId`     | `string` | SPL Token program pubkey for this account, when the validator recorded it. This field may be omitted. |

**`token balance.uiTokenAmount` fields**

| Field            | Type             | Description                                                                                |
| ---------------- | ---------------- | ------------------------------------------------------------------------------------------ |
| `uiAmount`       | `number \| null` | Decimal-scaled amount as a floating-point number. Deprecated in favor of `uiAmountString`. |
| `decimals`       | `u8`             | Number of decimal places configured on the mint.                                           |
| `amount`         | `string`         | Raw token amount, as a base-10 integer string.                                             |
| `uiAmountString` | `string`         | Decimal-scaled amount as a string.                                                         |

**`reward` fields**

| Field         | Type             | Description                                                                                     |
| ------------- | ---------------- | ----------------------------------------------------------------------------------------------- |
| `pubkey`      | `string`         | Rewarded account pubkey, as a base-58 encoded string.                                           |
| `lamports`    | `i64`            | Lamports credited or debited by this reward entry.                                              |
| `postBalance` | `u64`            | Account balance after the reward was applied.                                                   |
| `rewardType`  | `string \| null` | Reward category. When present, the value is `fee`, `rent`, `voting`, or `staking`.              |
| `commission`  | `u8 \| null`     | Vote-account commission at the time of the reward. Present only for voting and staking rewards. |

**`slot`** · u64

The slot this transaction was processed in

**`transaction`** · object | string | \[string,encoding]

Transaction payload. The shape depends on the `encoding` parameter:

For the shared transaction object variants, see [Transactions](https://solana.com/docs/rpc/json-structures#transactions).

| Encoding                     | Returned Structure                                   |
| ---------------------------- | ---------------------------------------------------- |
| `json`                       | Object with `signatures` and raw `message` fields    |
| `jsonParsed`                 | Object with `signatures` and parsed `message` fields |
| `base64`, `base58`, `binary` | Encoded transaction data                             |

When `encoding` is `json`, the top-level transaction object may contain:

| Field        | Type     | Description                           |
| ------------ | -------- | ------------------------------------- |
| `signatures` | `array`  | Transaction signatures in wire order. |
| `message`    | `object` | Serialized message content.           |

**`message` fields**

| Field                 | Type            | Description                                                                              |
| --------------------- | --------------- | ---------------------------------------------------------------------------------------- |
| `header`              | `object`        | Message header object.                                                                   |
| `accountKeys`         | `array`         | Ordered list of account addresses referenced by the message, as base-58 encoded strings. |
| `recentBlockhash`     | `string`        | Recent blockhash referenced by the message.                                              |
| `instructions`        | `array`         | Compiled instructions in the message.                                                    |
| `addressTableLookups` | `array \| null` | Address lookup table references for versioned transactions, when present.                |

**`message.accountKeys[]` fields when `encoding` is `jsonParsed`**

| Field      | Type             | Description                                                                           |
| ---------- | ---------------- | ------------------------------------------------------------------------------------- |
| `pubkey`   | `string`         | Account address, as a base-58 encoded string.                                         |
| `signer`   | `bool`           | Whether this account signed the transaction.                                          |
| `writable` | `bool`           | Whether the message marks this account writable.                                      |
| `source`   | `string \| null` | Source of the account key. When present, the value is `transaction` or `lookupTable`. |

**`message.header` fields**

| Field                         | Type | Description                           |
| ----------------------------- | ---- | ------------------------------------- |
| `numRequiredSignatures`       | `u8` | Number of required signatures.        |
| `numReadonlySignedAccounts`   | `u8` | Number of readonly signer accounts.   |
| `numReadonlyUnsignedAccounts` | `u8` | Number of readonly unsigned accounts. |

**`message.instructions[]` fields**

| Field            | Type          | Description                                       |
| ---------------- | ------------- | ------------------------------------------------- |
| `programIdIndex` | `u8`          | Index into `accountKeys` for the invoked program. |
| `accounts`       | `array`       | Account indexes referenced by the instruction.    |
| `data`           | `string`      | Instruction data, base-58 encoded.                |
| `stackHeight`    | `u32 \| null` | Invocation stack height, when reported.           |

**`message.instructions[]` parsed form fields**

| Field         | Type          | Description                                                                           |
| ------------- | ------------- | ------------------------------------------------------------------------------------- |
| `program`     | `string`      | Parser name that decoded the instruction.                                             |
| `programId`   | `string`      | Invoked program pubkey, as a base-58 encoded string.                                  |
| `parsed`      | `object`      | Program-specific parsed instruction payload. The exact schema depends on the program. |
| `stackHeight` | `u32 \| null` | Invocation stack height, when reported.                                               |

**`message.instructions[]` partially decoded form fields**

| Field         | Type          | Description                                                |
| ------------- | ------------- | ---------------------------------------------------------- |
| `programId`   | `string`      | Invoked program pubkey, as a base-58 encoded string.       |
| `accounts`    | `array`       | Account addresses referenced by the instruction, in order. |
| `data`        | `string`      | Instruction data, base-58 encoded.                         |
| `stackHeight` | `u32 \| null` | Invocation stack height, when reported.                    |

**`message.addressTableLookups[]` fields**

| Field             | Type     | Description                                                       |
| ----------------- | -------- | ----------------------------------------------------------------- |
| `accountKey`      | `string` | Address lookup table account pubkey, as a base-58 encoded string. |
| `writableIndexes` | `array`  | Lookup-table indexes loaded as writable accounts.                 |
| `readonlyIndexes` | `array`  | Lookup-table indexes loaded as readonly accounts.                 |

When `message` is returned as the parsed form (`encoding: "jsonParsed"`), `accountKeys` is an array of objects containing the listed metadata and `instructions` is an array of parsed or partially decoded instruction objects. The nested `parsed` payload within those instruction objects remains program-specific.

**`version`** · "legacy" | number

Transaction version. This field is omitted if `maxSupportedTransactionVersion` is not set in the request params.

***

_Adapted from the_ [_Solana `getTransaction` reference_](https://solana.com/docs/rpc/http/gettransaction)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
