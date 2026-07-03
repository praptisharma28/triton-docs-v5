---
description: >-
  Simulates a signed transaction using the chain data available at the requested
  commitment, without broadcasting it.
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

# simulateTransaction

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "simulateTransaction",
  "params": [
    "AQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA...ZAAAAAAAAAA=",
    {
      "commitment": "confirmed",
      "encoding": "base64",
      "replaceRecentBlockhash": true
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

const base64Tx: Base64EncodedWireTransaction =
  "AQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA...ZAAAAAAAAAA=" as Base64EncodedWireTransaction;

let simulateTxConfig = {
  commitment: "finalized",
  encoding: "base64",
  replaceRecentBlockhash: true,
  sigVerify: false,
  minContextSlot: undefined,
  innerInstructions: undefined,
  accounts: undefined
};

let simulateResult = await rpc
  .simulateTransaction(base64Tx, simulateTxConfig)
  .send();

console.log(simulateResult);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import {
  Connection,
  VersionedTransaction,
  type SimulateTransactionConfig
} from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

const base64Tx =
  "AQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA...ZAAAAAAAAAA=";

let tx = VersionedTransaction.deserialize(Buffer.from(base64Tx, "base64"));

let simulateTxConfig: SimulateTransactionConfig = {
  commitment: "finalized",
  replaceRecentBlockhash: true,
  sigVerify: false,
  minContextSlot: undefined,
  innerInstructions: undefined,
  accounts: undefined
};

let simulateResult = await connection.simulateTransaction(tx, simulateTxConfig);

console.log(simulateResult);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use base64::{Engine as _, engine::general_purpose};
use solana_client::{nonblocking::rpc_client::RpcClient, rpc_config::RpcSimulateTransactionConfig};
use solana_commitment_config::CommitmentConfig;
use solana_sdk::transaction::VersionedTransaction;
use solana_transaction_status_client_types::UiTransactionEncoding;

#[tokio::main]
async fn main() -> Result<()> {
    let client = RpcClient::new_with_commitment(
        String::from("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>"),
        CommitmentConfig::confirmed(),
    );

    let b64_tx = "AQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA...ZAAAAAAAAAA=";
    let tx_bytes = general_purpose::STANDARD.decode(b64_tx).unwrap();
    let tx: VersionedTransaction = bincode::deserialize(&tx_bytes).unwrap();

    let config = RpcSimulateTransactionConfig {
        commitment: CommitmentConfig::finalized().into(),
        encoding: UiTransactionEncoding::Base64.into(),
        replace_recent_blockhash: true,
        sig_verify: false,
        min_context_slot: None,
        inner_instructions: false,
        accounts: None,
    };

    let simulate_result = client.simulate_transaction_with_config(&tx, config).await?;

    println!("{:#?}", simulate_result);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`transaction`** · string · required

Transaction, as an encoded string.

{% hint style="info" %}
The transaction must include a recent blockhash unless `replaceRecentBlockhash` is `true`, in which case the RPC node replaces it before simulation. The transaction is not required to be signed unless `sigVerify` is `true`.
{% endhint %}

**`config`** · object · optional

Configuration object containing the following fields:

**`commitment`** · string · default `finalized`

Values: `processed`, `confirmed`, `finalized`

Commitment level to simulate the transaction at. Default `finalized`.

Solana RPC uses the following commitment levels:

| Value       | Description                                                                                                                                                                                                                                                                |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `processed` | Return data from the highest slot this node has processed on the fork it currently considers best. This is the newest view, but it can still change if the cluster switches forks.                                                                                         |
| `confirmed` | Return data from the highest slot that at least two-thirds of active stake has directly voted to confirm. This is more stable than `processed`, but it is still a weaker guarantee than `finalized`.                                                                       |
| `finalized` | Return data from the highest slot that the cluster recognizes as finalized. In practice, this means the slot has reached maximum vote lockout in validators' vote towers and is recognized by at least two-thirds of active stake. This is the strongest commitment level. |

**`encoding`** · string · default `base58`

Values: `binary`, `base58`, `base64`

Encoding used for the transaction data.

| Encoding | Notes                                                                          |
| -------- | ------------------------------------------------------------------------------ |
| `base64` | Recommended                                                                    |
| `base58` | Slow                                                                           |
| `binary` | ⚠️ Deprecated. Legacy alias for `base58`, retained for backwards compatibility |

**`replaceRecentBlockhash`** · bool · default `false`

Values: `true`, `false`

If `true` the transaction recent blockhash will be replaced with the most recent blockhash (conflicts with `sigVerify`)

**`sigVerify`** · bool · default `false`

Values: `true`, `false`

If `true` the transaction signatures will be verified (conflicts with `replaceRecentBlockhash`)

**`minContextSlot`** · number · optional

The minimum slot that the request can be evaluated at

**`innerInstructions`** · bool · default `false`

Values: `true`, `false`

If `true` the response will include CPI instructions (`innerInstructions`). These inner instructions will be `jsonParsed` where possible, otherwise `json`.

See [Inner Instructions](https://solana.com/docs/rpc/json-structures#inner-instructions) for the shared structure.

**`accounts`** · object · optional

Accounts configuration object containing the following fields:

| Field       | Type     | Description                                                                                                                                                                                                                                                                                                                                                                                                               |
| ----------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `addresses` | `array`  | Account addresses to return, as base-58 encoded strings. The list length must not exceed the number of account keys in the transaction.                                                                                                                                                                                                                                                                                   |
| `encoding`  | `string` | Encoding for returned account data. Default is `base64`. Supported values are `base64`, `base64+zstd`, and `jsonParsed`. `base58` and `binary` are not supported for this method. If `jsonParsed` is requested, each returned account uses the stable wrapper `{program, parsed, space}`; if no parser is available, the RPC node falls back to `[data, "base64"]`. The nested `parsed` payload remains program-specific. |

If `accounts.encoding` is `jsonParsed`, the `program` field in each returned account identifies the parser that produced `parsed`.

For the shared account wrapper used by these entries, see [Account Data](https://solana.com/docs/rpc/json-structures#account-data).

| `program` value          | Parsed account type                            |
| ------------------------ | ---------------------------------------------- |
| `address-lookup-table`   | Address lookup table accounts                  |
| `bpf-upgradeable-loader` | Upgradeable loader program and buffer accounts |
| `config`                 | Config accounts                                |
| `nonce`                  | Durable nonce accounts                         |
| `spl-token`              | SPL Token accounts                             |
| `spl-token-2022`         | SPL Token 2022 accounts                        |
| `stake`                  | Stake accounts                                 |
| `sysvar`                 | Sysvar accounts                                |
| `vote`                   | Vote accounts                                  |

## Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "apiVersion": "3.1.8",
      "slot": 393226680
    },
    "value": {
      "accounts": null,
      "err": null,
      "innerInstructions": null,
      "loadedAccountsDataSize": 413,
      "logs": [
        "Program TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb invoke [1]",
        "Program log: Instruction: Transfer",
        "Program TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb consumed 1714 of 200000 compute units",
        "Program TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb success"
      ],
      "replacementBlockhash": {
        "blockhash": "6oFLsE7kmgJx9PjR4R63VRNtpAVJ648gCTr3nq5Hihit",
        "lastValidBlockHeight": 381186895
      },
      "returnData": null,
      "unitsConsumed": 1714,
      "fee": 5000,
      "preBalances": [],
      "postBalances": [],
      "preTokenBalances": [],
      "postTokenBalances": [],
      "loadedAddresses": {
        "readonly": [],
        "writable": []
      }
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

Simulation result object containing:

This value uses the shared [Simulation Results](https://solana.com/docs/rpc/json-structures#simulation-results) structure.

| Field                    | Type                       | Description                                                                                                                                                            |
| ------------------------ | -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `accounts`               | `array \| null`            | Accounts requested in `accounts.addresses`. Each element is either `null` or an [Account Data](https://solana.com/docs/rpc/json-structures#account-data) entry.        |
| `err`                    | `object \| string \| null` | Error if the transaction failed. `null` if the transaction succeeded. See [Transaction Errors](https://solana.com/docs/rpc/json-structures#transaction-errors).        |
| `innerInstructions`      | `array \| null`            | `null` unless `innerInstructions` was `true`. Otherwise this is a list of [Inner Instructions](https://solana.com/docs/rpc/json-structures#inner-instructions) groups. |
| `loadedAccountsDataSize` | `u32 \| null`              | Total number of bytes loaded across all accounts used during simulation.                                                                                               |
| `logs`                   | `array \| null`            | Program log messages emitted during execution. `null` if simulation failed before execution began.                                                                     |
| `replacementBlockhash`   | `object \| null`           | Blockhash the RPC node used during simulation when `replaceRecentBlockhash` is enabled.                                                                                |
| `returnData`             | `object \| null`           | Most recent instruction return data captured during simulation, when present. See [Return Data](https://solana.com/docs/rpc/json-structures#return-data).              |
| `unitsConsumed`          | `u64 \| null`              | Compute units consumed during simulation.                                                                                                                              |
| `fee`                    | `u64 \| null`              | Fee charged for the simulated transaction.                                                                                                                             |
| `preBalances`            | `array \| null`            | Lamport balances before simulation, indexed to the transaction account list.                                                                                           |
| `postBalances`           | `array \| null`            | Lamport balances after simulation, indexed to the transaction account list.                                                                                            |
| `preTokenBalances`       | `array \| null`            | Token balances before simulation. Each element uses [Token Balances](https://solana.com/docs/rpc/json-structures#token-balances).                                      |
| `postTokenBalances`      | `array \| null`            | Token balances after simulation. Each element uses [Token Balances](https://solana.com/docs/rpc/json-structures#token-balances).                                       |
| `loadedAddresses`        | `object \| null`           | Transaction addresses loaded from address lookup tables, when present. See [Loaded Addresses](https://solana.com/docs/rpc/json-structures#loaded-addresses).           |

**`accounts[]` fields**

| Field        | Type                           | Description                                                                                                                                                                                                             |
| ------------ | ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `data`       | `[string, encoding] \| object` | Account data. Binary forms depend on `accounts.encoding`. When `accounts.encoding` is `jsonParsed`, this field is `{program, parsed, space}`; if no parser is available, the RPC node falls back to `[data, "base64"]`. |
| `executable` | `bool`                         | Whether the account contains a program and is therefore read-only.                                                                                                                                                      |
| `lamports`   | `u64`                          | Lamports assigned to the account.                                                                                                                                                                                       |
| `owner`      | `string`                       | Program owner pubkey, as a base-58 encoded string.                                                                                                                                                                      |
| `rentEpoch`  | `u64`                          | Next epoch at which the account owes rent.                                                                                                                                                                              |
| `space`      | `u64 \| null`                  | Account data size in bytes. This may be `null` if the selected encoding omits it.                                                                                                                                       |

When `accounts[].data` is returned as an object, it contains:

| Field     | Type     | Description                                                                                 |
| --------- | -------- | ------------------------------------------------------------------------------------------- |
| `program` | `string` | Name of the parser that produced the decoded account data.                                  |
| `parsed`  | `object` | Parser-specific JSON payload. Its structure depends on the owning program and account type. |
| `space`   | `u64`    | Account data size in bytes.                                                                 |

**`err` object form**

| Field           | Type                                | Description                                                                                                                                     |
| --------------- | ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| `<variantName>` | `array \| object \| string \| null` | Transaction error variant payload. Most variants serialize as strings. `InstructionError` serializes as `[instructionIndex, instructionError]`. |

**`innerInstructions[]` fields**

| Field          | Type    | Description                                                                           |
| -------------- | ------- | ------------------------------------------------------------------------------------- |
| `index`        | `u8`    | Zero-based index of the top-level instruction that produced these inner instructions. |
| `instructions` | `array` | Inner instructions emitted while processing that top-level instruction.               |

**`innerInstructions[].instructions[]` parsed form fields**

| Field         | Type          | Description                                                                           |
| ------------- | ------------- | ------------------------------------------------------------------------------------- |
| `program`     | `string`      | Parser name that decoded the instruction.                                             |
| `programId`   | `string`      | Invoked program pubkey, as a base-58 encoded string.                                  |
| `parsed`      | `object`      | Program-specific parsed instruction payload. The exact schema depends on the program. |
| `stackHeight` | `u32 \| null` | Invocation stack height, when reported.                                               |

**`innerInstructions[].instructions[]` partially decoded form fields**

| Field         | Type          | Description                                                |
| ------------- | ------------- | ---------------------------------------------------------- |
| `programId`   | `string`      | Invoked program pubkey, as a base-58 encoded string.       |
| `accounts`    | `array`       | Account addresses referenced by the instruction, in order. |
| `data`        | `string`      | Instruction data, base-58 encoded.                         |
| `stackHeight` | `u32 \| null` | Invocation stack height, when reported.                    |

**`replacementBlockhash` fields**

| Field                  | Type     | Description                                                    |
| ---------------------- | -------- | -------------------------------------------------------------- |
| `blockhash`            | `string` | Blockhash used during simulation, as a base-58 encoded string. |
| `lastValidBlockHeight` | `u64`    | Last block height at which that blockhash remains valid.       |

**`returnData` fields**

| Field       | Type                 | Description                                                                              |
| ----------- | -------------------- | ---------------------------------------------------------------------------------------- |
| `programId` | `string`             | Program that generated the return data, as a base-58 encoded Pubkey.                     |
| `data`      | `[string, encoding]` | Return data payload and encoding. This field uses base-64 data with `"base64"` encoding. |

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

**`loadedAddresses` fields**

| Field      | Type    | Description                                                                    |
| ---------- | ------- | ------------------------------------------------------------------------------ |
| `writable` | `array` | Ordered list of writable loaded-account addresses, as base-58 encoded strings. |
| `readonly` | `array` | Ordered list of readonly loaded-account addresses, as base-58 encoded strings. |

***

_Adapted from the_ [_Solana `simulateTransaction` reference_](https://solana.com/docs/rpc/http/simulatetransaction)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
