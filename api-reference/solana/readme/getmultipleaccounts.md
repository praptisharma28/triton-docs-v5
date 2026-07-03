---
description: Returns account state and metadata for multiple addresses, in request order.
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

# getMultipleAccounts

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getMultipleAccounts",
  "params": [
    [
      "vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg",
      "4fYNw3dojWmQ4dXtSGE9epjRGy9pFSx62YypT7avPYvA"
    ],
    {
      "encoding": "base64",
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

let addresses = [
  address("vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg"),
  address("4fYNw3dojWmQ4dXtSGE9epjRGy9pFSx62YypT7avPYvA")
];

let config = {
  encoding: "base64",
  commitment: "finalized"
};

let accounts = await rpc.getMultipleAccounts(addresses, config).send();

console.log(accounts);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import {
  Connection,
  PublicKey,
  type GetMultipleAccountsConfig
} from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

let addresses = [
  new PublicKey("vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg"),
  new PublicKey("4fYNw3dojWmQ4dXtSGE9epjRGy9pFSx62YypT7avPYvA")
];

let config: GetMultipleAccountsConfig = {
  commitment: "finalized"
};

let accounts = await connection.getMultipleAccountsInfo(addresses, config);

console.log(accounts);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use solana_account_decoder::UiAccountEncoding;
use solana_client::{nonblocking::rpc_client::RpcClient, rpc_config::RpcAccountInfoConfig};
use solana_commitment_config::CommitmentConfig;
use solana_sdk::pubkey;

#[tokio::main]
async fn main() -> Result<()> {
    let client = RpcClient::new_with_commitment(
        String::from("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>"),
        CommitmentConfig::confirmed(),
    );

    let addresses = [
        pubkey!("vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg"),
        pubkey!("4fYNw3dojWmQ4dXtSGE9epjRGy9pFSx62YypT7avPYvA"),
    ];

    let config = RpcAccountInfoConfig {
        encoding: UiAccountEncoding::Base64.into(),
        data_slice: None,
        commitment: CommitmentConfig::finalized().into(),
        min_context_slot: None,
    };

    let accounts = client
        .get_multiple_accounts_with_config(&addresses, config)
        .await?;

    println!("{:#?}", accounts);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`pubkeys`** · array · required

An array of Pubkeys to query, as base-58 encoded strings (up to a maximum of 100)

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

**`encoding`** · string · default `base64`

Values: `base58`, `base64`, `base64+zstd`, `binary`, `jsonParsed`

Encoding format for account data.

| Encoding      | Data format                | Notes                                                                                                                                                                                                                    |
| ------------- | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `base64`      | `[data, "base64"]`         | Recommended                                                                                                                                                                                                              |
| `base58`      | `[data, "base58"]`         | Slow. The account's data field must be 128 bytes or fewer                                                                                                                                                                |
| `base64+zstd` | `[data, "base64+zstd"]`    | [Zstandard](https://facebook.github.io/zstd/) compressed                                                                                                                                                                 |
| `jsonParsed`  | `{program, parsed, space}` | Falls back to `[data, "base64"]` if no parser found                                                                                                                                                                      |
| `binary`      | `string`                   | Deprecated. Legacy encoding retained for backwards compatibility. Same base58 encoding as `base58` with the same 128-byte data field limit, but returns data as a plain string instead of an array. Use `base64` instead |

If you use `jsonParsed`, each returned account uses the stable wrapper `{program, parsed, space}`. The `program` field identifies the parser that produced `parsed`. If no parser is available, the RPC node falls back to `[data, "base64"]`.

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

The nested `parsed` payload remains program-specific.

For the shared account wrapper returned by this method, see [Account Data](https://solana.com/docs/rpc/json-structures#account-data).

## Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": { "apiVersion": "3.1.8", "slot": 341197247 },
    "value": [
      {
        "data": ["", "base64"],
        "executable": false,
        "lamports": 88849814690250,
        "owner": "11111111111111111111111111111111",
        "rentEpoch": 18446744073709551615,
        "space": 0
      },
      {
        "data": ["", "base64"],
        "executable": false,
        "lamports": 998763433,
        "owner": "2WRuhE4GJFoE23DYzp2ij6ZnuQ8p9mJeU6gDgfsjR4or",
        "rentEpoch": 18446744073709551615,
        "space": 0
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

Array containing either:

* `null` - if the account at that Pubkey doesn't exist, or
* an account object with the following fields:

Each non-null element uses the shared [Account Data](https://solana.com/docs/rpc/json-structures#account-data) structure.

| Field        | Type                                     | Description                                                                                                                                                                                                                                                                                              |
| ------------ | ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `data`       | `string \| [string, encoding] \| object` | Account data. If `encoding` is `binary`, this is a plain base-58 string. If `encoding` is `base58`, `base64`, or `base64+zstd`, this is `[data, encoding]`. If `encoding` is `jsonParsed`, this is `{program, parsed, space}`; if no parser is available, the RPC node falls back to `[data, "base64"]`. |
| `executable` | `bool`                                   | Whether the account contains a program and is therefore read-only.                                                                                                                                                                                                                                       |
| `lamports`   | `u64`                                    | Lamports assigned to this account.                                                                                                                                                                                                                                                                       |
| `owner`      | `string`                                 | Program owner pubkey, as a base-58 encoded string.                                                                                                                                                                                                                                                       |
| `rentEpoch`  | `u64`                                    | Epoch at which this account next owes rent.                                                                                                                                                                                                                                                              |
| `space`      | `u64 \| null`                            | Account data size in bytes. Current Solana RPC responses populate this field for encoded account responses.                                                                                                                                                                                              |

When `data` is returned as an object, it contains:

| Field     | Type     | Description                                                                                 |
| --------- | -------- | ------------------------------------------------------------------------------------------- |
| `program` | `string` | Name of the parser that produced the decoded account data.                                  |
| `parsed`  | `object` | Parser-specific JSON payload. Its structure depends on the owning program and account type. |
| `space`   | `u64`    | Account data size in bytes.                                                                 |

***

_Adapted from the_ [_Solana `getMultipleAccounts` reference_](https://solana.com/docs/rpc/http/getmultipleaccounts)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
