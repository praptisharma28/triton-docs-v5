---
description: >-
  Returns a confirmed block for a slot, including block metadata and transaction
  data in the requested encoding.
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

# getBlock

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getBlock",
  "params": [
    378967388,
    {
      "commitment": "finalized",
      "encoding": "json",
      "transactionDetails": "full",
      "maxSupportedTransactionVersion": 0,
      "rewards": true
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

const slot_number = BigInt(377261141);

let block = await rpc
  .getBlock(
    slot_number,
    {
      commitment: "finalized",
      encoding: "json",
      transactionDetails: "full",
      maxSupportedTransactionVersion: 0,
      rewards: true
    }
  )
  .send();

console.log("block:", block);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import { Connection } from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

const slot_number = 377261141;

const block = await connection.getBlock(
  slot_number,
  {
    commitment: "finalized",
    transactionDetails: "full",
    maxSupportedTransactionVersion: 0,
    rewards: true
  }
);

console.log("block:", block);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use solana_client::nonblocking::rpc_client::RpcClient;
use solana_commitment_config::CommitmentConfig;
use solana_transaction_status_client_types::{TransactionDetails, UiTransactionEncoding};

#[tokio::main]
async fn main() -> Result<()> {
    let client = RpcClient::new_with_commitment(
        String::from("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>"),
        CommitmentConfig::confirmed(),
    );

    let slot_number = 377261141;

    let config = solana_client::rpc_config::RpcBlockConfig {
        encoding: UiTransactionEncoding::Json.into(),
        transaction_details: TransactionDetails::Full.into(),
        rewards: None,
        commitment: CommitmentConfig::finalized().into(),
        max_supported_transaction_version: Some(0),
    };
    let block = client.get_block_with_config(slot_number, config).await?;

    println!("Block: {:#?}", block);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`slot number`** · u64 · required

Slot number.

**`config`** · string | object · optional

Configuration object. For backwards compatibility, this parameter also accepts a bare encoding string, but prefer the object form.

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

**`encoding`** · string · default `json`

Values: `binary`, `base58`, `base64`, `json`, `jsonParsed`

Encoding format for each returned transaction.

| Encoding     | Transaction format | Notes                                                                                                                                                              |
| ------------ | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `json`       | JSON object        | Default. Returns parsed transaction with `message` and `signatures`                                                                                                |
| `jsonParsed` | JSON object        | Uses program-specific parsers for `transaction.message.instructions`. Falls back to standard JSON fields (`accounts`, `data`, `programIdIndex`) if no parser found |
| `base64`     | `[data, "base64"]` | Base64 encoded binary transaction                                                                                                                                  |
| `base58`     | `[data, "base58"]` | Slow                                                                                                                                                               |
| `binary`     | `[data, "base58"]` | ⚠️ Deprecated. Legacy alias for `base58`, retained for backwards compatibility                                                                                     |

For `jsonParsed`, instruction entries use one of two stable outer shapes: `{program, programId, parsed, stackHeight}` when the RPC node can parse the instruction, or `{programId, accounts, data, stackHeight}` when it falls back to a partially decoded form. The nested `parsed` payload remains program-specific.

If you use the deprecated shorthand encoding form, pass one of these encoding strings directly.

For the shared JSON structures used by `json` and `jsonParsed` responses, see [Transactions](https://solana.com/docs/rpc/json-structures#transactions), [Parsed Accounts](https://solana.com/docs/rpc/json-structures#parsed-accounts), [Inner Instructions](https://solana.com/docs/rpc/json-structures#inner-instructions), and [Address Table Lookups](https://solana.com/docs/rpc/json-structures#address-table-lookups).

**`transactionDetails`** · string · default `full`

Values: `full`, `accounts`, `signatures`, `none`

Level of transaction detail to return.

* If `accounts` is requested, the response still includes `transactions`, but each `transactions[].transaction` object only contains `signatures` and an annotated `accountKeys` list.
* In `accounts` mode, `transactions[].meta` is limited to `fee`, `err`, `status`, `preBalances`, `postBalances`, `preTokenBalances`, `postTokenBalances`, and `rewards`.
* If `signatures` is requested, the response omits `transactions` and returns only the `signatures` array for the block.
* If `none` is requested, the response omits both `transactions` and `signatures`.

**`maxSupportedTransactionVersion`** · number

Values: `0`

Currently, the only valid value for this parameter is `0`. Setting it to `0` allows you to fetch all transactions, including both Versioned and legacy transactions.

This parameter determines the maximum transaction version that will be returned in the response. If you request a transaction with a higher version than this value, an error will be returned. If you omit this parameter, only legacy transactions will be returned; any block containing a versioned transaction will result in an error.

**`rewards`** · bool · default `true`

Values: `true`, `false`

Whether to populate the rewards array. If parameter not provided, the default includes rewards.

## Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "blockHeight": 428,
    "blockTime": null,
    "blockhash": "3Eq21vXNB5s86c62bVuUfTeaMif1N2kUqRPBmGRJhyTA",
    "parentSlot": 429,
    "previousBlockhash": "mfcyqEXB3DnHXki6KjjmZck6YjmZLvpAByy2fj4nh6B",
    "rewards": [],
    "numRewardPartitions": null,
    "transactions": [
      {
        "meta": {
          "err": null,
          "fee": 5000,
          "innerInstructions": [],
          "logMessages": [],
          "postBalances": [499998932500, 26858640, 1, 1, 1],
          "postTokenBalances": [],
          "preBalances": [499998937500, 26858640, 1, 1, 1],
          "preTokenBalances": [],
          "rewards": null,
          "status": {
            "Ok": null
          }
        },
        "transaction": {
          "message": {
            "accountKeys": [
              "3UVYmECPPMZSCqWKfENfuoTv51fTDTWicX9xmBD2euKe",
              "AjozzgE83A3x1sHNUR64hfH7zaEBWeMaFuAN9kQgujrc",
              "SysvarS1otHashes111111111111111111111111111",
              "SysvarC1ock11111111111111111111111111111111",
              "Vote111111111111111111111111111111111111111"
            ],
            "header": {
              "numReadonlySignedAccounts": 0,
              "numReadonlyUnsignedAccounts": 3,
              "numRequiredSignatures": 1
            },
            "instructions": [
              {
                "accounts": [1, 2, 3, 0],
                "data": "37u9WtQpcm6ULa3WRQHmj49EPs4if7o9f1jSRVZpm2dvihR9C8jY4NqEwXUbLwx15HBSNcP1",
                "programIdIndex": 4
              }
            ],
            "recentBlockhash": "mfcyqEXB3DnHXki6KjjmZck6YjmZLvpAByy2fj4nh6B"
          },
          "signatures": [
            "2nBhEBYYvfaAe16UMNqRHre4YNSskvuYgx3M6E4JP1oDYvZEJHvoPzyUidNgNX5r9sTyN1J9UxtbCXy2rqYcuyuv"
          ]
        }
      }
    ]
  },
  "id": 1
}
```

## Result

If the specified block is not confirmed, the result is `null`. Otherwise, an object containing:

This top-level response uses the shared [Confirmed Blocks](https://solana.com/docs/rpc/json-structures#confirmed-blocks) structure.

**`blockHeight`** · u64 | null

The number of blocks beneath this block.

**`blockTime`** · i64 | null

Estimated production time, as Unix timestamp (seconds since the Unix epoch). `null` if not available.

**`blockhash`** · string

The blockhash of this block, as base-58 encoded string

**`parentSlot`** · u64

The slot index of this block's parent

**`previousBlockhash`** · string

The blockhash of this block's parent, as base-58 encoded string; if the parent block is not available due to ledger cleanup, this field will return "11111111111111111111111111111111"

**`transactions`** · array

Present if `transactionDetails` is `"full"` or `"accounts"`; an array of JSON objects containing:

| Field         | Type                                     | Description                                                                                                                                                                                                                                                                                                                                                                       |
| ------------- | ---------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `transaction` | `object \| string \| [string, encoding]` | Encoded transaction payload. For `transactionDetails: "full"`, this uses the shared [Encoded Transaction Responses](https://solana.com/docs/rpc/json-structures#encoded-transaction-responses) structure. For `transactionDetails: "accounts"`, this uses [Accounts Only Transaction Responses](https://solana.com/docs/rpc/json-structures#accounts-only-transaction-responses). |
| `meta`        | `object \| null`                         | Transaction status metadata, or `null` if metadata is unavailable. For `transactionDetails: "full"`, this uses [Transaction Status Metadata](https://solana.com/docs/rpc/json-structures#transaction-status-metadata). For `transactionDetails: "accounts"`, current Solana RPC responses return a simplified subset of that structure.                                           |
| `version`     | `"legacy" \| number`                     | Transaction version. This field is omitted if `maxSupportedTransactionVersion` is not set in the request params.                                                                                                                                                                                                                                                                  |

**`transactions[].transaction` fields when `transactionDetails` is `"accounts"`**

| Field         | Type    | Description                                                                                             |
| ------------- | ------- | ------------------------------------------------------------------------------------------------------- |
| `signatures`  | `array` | Transaction signatures in wire order.                                                                   |
| `accountKeys` | `array` | Objects containing the listed `pubkey`, `signer`, and `writable` metadata for each transaction account. |

**`transactions[].transaction.accountKeys[]` fields when `transactionDetails` is `"accounts"`**

| Field      | Type             | Description                                                                           |
| ---------- | ---------------- | ------------------------------------------------------------------------------------- |
| `pubkey`   | `string`         | Account address, as a base-58 encoded string.                                         |
| `signer`   | `bool`           | Whether this account signed the transaction.                                          |
| `writable` | `bool`           | Whether the transaction marks this account writable.                                  |
| `source`   | `string \| null` | Source of the account key. When present, the value is `transaction` or `lookupTable`. |

**`transactions[].meta` fields when `transactionDetails` is `"accounts"`**

| Field               | Type                       | Description                                                                                                                         |
| ------------------- | -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `err`               | `object \| string \| null` | Transaction error. `null` indicates success.                                                                                        |
| `status`            | `object`                   | Deprecated status object that mirrors `err`. It is `"Ok": <null>` for success or `"Err": <ERR>` for failure.                        |
| `fee`               | `u64`                      | Fee charged for the transaction, in lamports.                                                                                       |
| `preBalances`       | `array`                    | Lamport balances before execution, indexed to `transaction.accountKeys`.                                                            |
| `postBalances`      | `array`                    | Lamport balances after execution, indexed to `transaction.accountKeys`.                                                             |
| `preTokenBalances`  | `array \| null`            | Token balances before execution. Each element uses [Token Balances](https://solana.com/docs/rpc/json-structures#token-balances).    |
| `postTokenBalances` | `array \| null`            | Token balances after execution. Each element uses [Token Balances](https://solana.com/docs/rpc/json-structures#token-balances).     |
| `rewards`           | `array \| null`            | Rewards applied while processing the transaction. Each element uses [Rewards](https://solana.com/docs/rpc/json-structures#rewards). |

**`transactions[].meta.err` object form**

| Field           | Type                                | Description                                                                                                                                     |
| --------------- | ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| `<variantName>` | `array \| object \| string \| null` | Transaction error variant payload. Most variants serialize as strings. `InstructionError` serializes as `[instructionIndex, instructionError]`. |

**`token balance` fields**

| Field           | Type     | Description                                                                                           |
| --------------- | -------- | ----------------------------------------------------------------------------------------------------- |
| `accountIndex`  | `u8`     | Index into the transaction account list for the token account whose balance is reported.              |
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

**`signatures`** · array

Present if "signatures" transaction details are requested; an array of signature strings, corresponding to the transaction order in the block.

**`rewards`** · array

Present if rewards are requested; an array of reward objects with the following fields:

This field uses the shared [Rewards](https://solana.com/docs/rpc/json-structures#rewards) structure.

| Field         | Type             | Description                                                                                     |
| ------------- | ---------------- | ----------------------------------------------------------------------------------------------- |
| `pubkey`      | `string`         | Rewarded account pubkey, as a base-58 encoded string.                                           |
| `lamports`    | `i64`            | Lamports credited or debited by this reward entry.                                              |
| `postBalance` | `u64`            | Account balance after the reward was applied.                                                   |
| `rewardType`  | `string \| null` | Reward category. When present, the value is `fee`, `rent`, `voting`, or `staking`.              |
| `commission`  | `u8 \| null`     | Vote-account commission at the time of the reward. Present only for voting and staking rewards. |

**`numRewardPartitions`** · u64 | null

The number of reward partitions for this block if using partitioned rewards, or `null`.

***

_Adapted from the_ [_Solana `getBlock` reference_](https://solana.com/docs/rpc/http/getblock)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
