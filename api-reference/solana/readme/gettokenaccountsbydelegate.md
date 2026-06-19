---
description: >-
  Returns SPL Token accounts whose approved delegate matches the supplied
  address.
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

# getTokenAccountsByDelegate

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getTokenAccountsByDelegate",
  "params": [
    "9mMBg1qaUaQdht6Q7r5T5VirRzJbNAyquKTq96FyR4UC",
    {
      "mint": "Gh9ZwEmdLJ8DscKNTkTqPbNwLNNBjuSzaG9Vp2KGtKJr"
    },
    {
      "commitment": "finalized",
      "encoding": "jsonParsed"
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

let delegate = address("4kg8oh3jdNtn7j2wcS7TrUua31AgbLzDVkBZgTAe44aF");

let tokenProgram = address("TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA");

let tokenAccByDelegate = await rpc
  .getTokenAccountsByDelegate(
    delegate,
    { programId: tokenProgram },
    {
      commitment: "finalized",
      encoding: "jsonParsed"
    }
  )
  .send();

console.log(tokenAccByDelegate);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use solana_client::{nonblocking::rpc_client::RpcClient, rpc_request::TokenAccountsFilter};
use solana_commitment_config::CommitmentConfig;
use solana_sdk::pubkey;

#[tokio::main]
async fn main() -> Result<()> {
    let client = RpcClient::new_with_commitment(
        String::from("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>"),
        CommitmentConfig::confirmed(),
    );

    let delegate = pubkey!("4kg8oh3jdNtn7j2wcS7TrUua31AgbLzDVkBZgTAe44aF");

    let token_program = pubkey!("TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA");

    let token_acc_by_del = client
        .get_token_accounts_by_delegate_with_commitment(
            &delegate,
            TokenAccountsFilter::ProgramId(token_program),
            CommitmentConfig::finalized(),
        )
        .await?;

    println!("{:#?}", token_acc_by_del);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`pubkey`** · string · required

Pubkey of account delegate to query, as base-58 encoded string

**`token account filter`** · object · required

A JSON object with one of the following fields:

**`mint`** · string

Pubkey of the specific token Mint to limit accounts to, as base-58 encoded string; or

**`programId`** · string

Pubkey of the Token program that owns the accounts, as base-58 encoded string

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

**`minContextSlot`** · number

The minimum slot that the request can be evaluated at

**`dataSlice`** · object

Request a slice of the account's data.

| Field    | Type    | Description                                           |
| -------- | ------- | ----------------------------------------------------- |
| `offset` | `usize` | Byte offset from which to start reading account data. |
| `length` | `usize` | Number of bytes to return.                            |

{% hint style="info" %}
Data slicing is only available for `base58`, `base64`, `base64+zstd`, and `binary` encodings.
{% endhint %}

{% hint style="info" %}
Use `encoding: "jsonParsed"` if you want the parsed token account structure shown in the response example below.
{% endhint %}

**`encoding`** · string · default `binary`

Values: `base58`, `base64`, `base64+zstd`, `binary`, `jsonParsed`

Encoding format for account data.

| Encoding      | Data format                | Notes                                                                                                                                                                                                                    |
| ------------- | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `base64`      | `[data, "base64"]`         | Recommended                                                                                                                                                                                                              |
| `base58`      | `[data, "base58"]`         | Slow. The account's data field must be 128 bytes or fewer                                                                                                                                                                |
| `base64+zstd` | `[data, "base64+zstd"]`    | [Zstandard](https://facebook.github.io/zstd/) compressed                                                                                                                                                                 |
| `jsonParsed`  | `{program, parsed, space}` | Falls back to `[data, "base64"]` if no parser found                                                                                                                                                                      |
| `binary`      | `string`                   | Deprecated. Legacy encoding retained for backwards compatibility. Same base58 encoding as `base58` with the same 128-byte data field limit, but returns data as a plain string instead of an array. Use `base64` instead |

If you use `jsonParsed`, this method returns the token-account wrapper `{program, parsed, space}`. The `program` field identifies the token parser that produced `parsed`.

| `program` value  | Parsed account type     |
| ---------------- | ----------------------- |
| `spl-token`      | SPL Token accounts      |
| `spl-token-2022` | SPL Token 2022 accounts |

The nested `parsed` payload on this page uses the SPL Token account form documented below.

The nested `account` field uses the shared [Account Data](https://solana.com/docs/rpc/json-structures#account-data) wrapper, and `tokenAmount` uses the same amount fields documented in [Token Balances](https://solana.com/docs/rpc/json-structures#token-balances).

## Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": { "apiVersion": "3.1.8", "slot": 1114 },
    "value": [
      {
        "pubkey": "28YTZEwqtMHWrhWcvv34se7pjS7wctgqzCPB3gReCFKp",
        "account": {
          "data": {
            "program": "spl-token",
            "parsed": {
              "info": {
                "tokenAmount": {
                  "amount": "1",
                  "decimals": 1,
                  "uiAmount": 0.1,
                  "uiAmountString": "0.1"
                },
                "delegate": "4Nd1mBQtrMJVYVfKf2PJy9NZUZdTAsp7D4xWLs4gDB4T",
                "delegatedAmount": {
                  "amount": "1",
                  "decimals": 1,
                  "uiAmount": 0.1,
                  "uiAmountString": "0.1"
                },
                "state": "initialized",
                "isNative": false,
                "mint": "3wyAj7Rt1TWVPZVteFJPLa26JmLvdb1CAKEFZm3NY75E",
                "owner": "CnPoSPKXu7wJqxe59Fs72tkBeALovhsCxYeFwPCQH9TD"
              },
              "type": "account"
            },
            "space": 165
          },
          "executable": false,
          "lamports": 1726080,
          "owner": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
          "rentEpoch": 4,
          "space": 165
        }
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

Array of keyed-account objects with the following fields:

| Field     | Type     | Description                                                                                                              |
| --------- | -------- | ------------------------------------------------------------------------------------------------------------------------ |
| `pubkey`  | `string` | Account pubkey, as a base-58 encoded string.                                                                             |
| `account` | `object` | Account data object. Uses the shared [Account Data](https://solana.com/docs/rpc/json-structures#account-data) structure. |

| Field        | Type                                     | Description                                                                                                                                                                                                               |
| ------------ | ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `data`       | `string \| [string, encoding] \| object` | Token account data. Binary forms depend on the `encoding` parameter. When `encoding` is `jsonParsed`, this field is `{program, parsed, space}`; if no parser is available, the RPC node falls back to `[data, "base64"]`. |
| `executable` | `bool`                                   | Whether the account contains a program and is therefore read-only.                                                                                                                                                        |
| `lamports`   | `u64`                                    | Lamports assigned to the account.                                                                                                                                                                                         |
| `owner`      | `string`                                 | Program owner pubkey, as a base-58 encoded string.                                                                                                                                                                        |
| `rentEpoch`  | `u64`                                    | Next epoch at which the account owes rent.                                                                                                                                                                                |
| `space`      | `u64 \| null`                            | Account data size in bytes. Current Solana RPC responses populate this field for encoded account responses.                                                                                                               |

When `account.data` is returned as an object, it contains:

| Field     | Type     | Description                                                                                                 |
| --------- | -------- | ----------------------------------------------------------------------------------------------------------- |
| `program` | `string` | Name of the parser that produced the decoded token-account data.                                            |
| `parsed`  | `object` | Parsed SPL Token account payload. For this method, this field uses the token-account form documented below. |
| `space`   | `u64`    | Account data size in bytes.                                                                                 |

**`parsed` fields**

| Field  | Type     | Description                       |
| ------ | -------- | --------------------------------- |
| `type` | `string` | Always `account` for this method. |
| `info` | `object` | Parsed SPL Token account fields.  |

**`parsed.info` fields**

| Field               | Type             | Description                                                                                                                       |
| ------------------- | ---------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| `mint`              | `string`         | Token mint pubkey, as a base-58 encoded string.                                                                                   |
| `owner`             | `string`         | Token-account owner pubkey, as a base-58 encoded string.                                                                          |
| `tokenAmount`       | `object`         | Token amount object with raw and decimal-scaled representations.                                                                  |
| `delegate`          | `string \| null` | Delegate pubkey, when a delegate is set.                                                                                          |
| `state`             | `string`         | Token-account state. The value is `uninitialized`, `initialized`, or `frozen`.                                                    |
| `isNative`          | `bool`           | Whether this account wraps the native SOL mint.                                                                                   |
| `rentExemptReserve` | `object \| null` | Rent-exempt reserve amount for wrapped SOL accounts, when present. Uses the same token-amount schema as `tokenAmount`.            |
| `delegatedAmount`   | `object \| null` | Amount delegated to `delegate`, when present. Uses the same token-amount schema as `tokenAmount`.                                 |
| `closeAuthority`    | `string \| null` | Close-authority pubkey, when present.                                                                                             |
| `extensions`        | `array`          | Parsed SPL Token 2022 extension entries. Each element has an `extension` discriminator and an extension-specific `state` payload. |

**`tokenAmount` fields**

This nested amount object uses the same numeric fields documented in [Token Balances](https://solana.com/docs/rpc/json-structures#token-balances).

| Field            | Type             | Description                                                                                |
| ---------------- | ---------------- | ------------------------------------------------------------------------------------------ |
| `uiAmount`       | `number \| null` | Decimal-scaled amount as a floating-point number. Deprecated in favor of `uiAmountString`. |
| `decimals`       | `u8`             | Number of decimal places configured on the mint.                                           |
| `amount`         | `string`         | Raw token amount, as a base-10 integer string.                                             |
| `uiAmountString` | `string`         | Decimal-scaled amount as a string.                                                         |

***

_Adapted from the_ [_Solana `getTokenAccountsByDelegate` reference_](https://solana.com/docs/rpc/http/gettokenaccountsbydelegate)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
