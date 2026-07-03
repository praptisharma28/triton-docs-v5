---
description: >-
  Returns the account state and metadata for a single address. Returns null if
  the account does not exist at the requested commitment.
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

# getAccountInfo

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getAccountInfo",
  "params": [
    "vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg",
    {
      "commitment": "finalized",
      "encoding": "base64"
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

const publicKey = address("vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg");
const accountInfo = await rpc.getAccountInfo(publicKey).send();

console.log("Account Info:", accountInfo);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import { Connection, PublicKey } from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");
const publicKey = new PublicKey("vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg");
const accountInfo = await connection.getAccountInfo(publicKey);

console.log("Account Info:", JSON.stringify(accountInfo, null, 2));
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use solana_client::nonblocking::rpc_client::RpcClient;
use solana_commitment_config::CommitmentConfig;
use solana_sdk::pubkey::Pubkey;
use std::str::FromStr;

#[tokio::main]
async fn main() -> Result<()> {
    let client = RpcClient::new_with_commitment(
        String::from("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>"),
        CommitmentConfig::confirmed(),
    );
    let pubkey = Pubkey::from_str("vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg")?;
    let account = client.get_account(&pubkey).await?;

    println!("{:#?}", account);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`pubkey`** · string · required

Pubkey of account to query, as base-58 encoded string.

**`config`** · object

Configuration object.

**`commitment`** · string · default `finalized`

Values: `processed`, `confirmed`, `finalized`

Solana RPC uses the following commitment levels:

| Value       | Description                                                                                                                                                                                                                                                                |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `processed` | Return data from the highest slot this node has processed on the fork it currently considers best. This is the newest view, but it can still change if the cluster switches forks.                                                                                         |
| `confirmed` | Return data from the highest slot that at least two-thirds of active stake has directly voted to confirm. This is more stable than `processed`, but it is still a weaker guarantee than `finalized`.                                                                       |
| `finalized` | Return data from the highest slot that the cluster recognizes as finalized. In practice, this means the slot has reached maximum vote lockout in validators' vote towers and is recognized by at least two-thirds of active stake. This is the strongest commitment level. |

**`encoding`** · string · default `binary`

Values: `base58`, `base64`, `base64+zstd`, `binary`, `jsonParsed`

Encoding format for account data.

| Encoding      | Data format                | Notes                                                                                                                                                                                                                       |
| ------------- | -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `base64`      | `[data, "base64"]`         | Recommended                                                                                                                                                                                                                 |
| `base58`      | `[data, "base58"]`         | Slow. The account's data field must be 128 bytes or fewer                                                                                                                                                                   |
| `base64+zstd` | `[data, "base64+zstd"]`    | [Zstandard](https://facebook.github.io/zstd/) compressed                                                                                                                                                                    |
| `jsonParsed`  | `{program, parsed, space}` | Falls back to `[data, "base64"]` if no parser found                                                                                                                                                                         |
| `binary`      | `string`                   | ⚠️ Deprecated. Legacy encoding retained for backwards compatibility. Same base58 encoding as `base58` with the same 128-byte data field limit, but returns data as a plain string instead of an array. Use `base64` instead |

If you use `jsonParsed`, the returned data object uses the stable wrapper `{program, parsed, space}`. The `program` field identifies the parser that produced `parsed`. If no parser is available, the RPC node falls back to `[data, "base64"]`.

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

**`dataSlice`** · object

Request a slice of the account's data.

| Field    | Type    | Description                                           |
| -------- | ------- | ----------------------------------------------------- |
| `offset` | `usize` | Byte offset from which to start reading account data. |
| `length` | `usize` | Number of bytes to return.                            |

{% hint style="info" %}
Data slicing is only available for `base58`, `base64`, `base64+zstd`, and `binary` encodings.
{% endhint %}

**`minContextSlot`** · number

The minimum slot that the request can be evaluated at.

## Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": { "apiVersion": "3.1.8", "slot": 341197053 },
    "value": {
      "data": ["", "base64"],
      "executable": false,
      "lamports": 88849814690250,
      "owner": "11111111111111111111111111111111",
      "rentEpoch": 18446744073709551615,
      "space": 0
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

**`value`** · object | null

If the requested account does not exist, `value` is `null`. Otherwise, it is an account object with the following fields:

This uses the shared [Account Data](https://solana.com/docs/rpc/json-structures#account-data) structure.

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

_Adapted from the_ [_Solana `getAccountInfo` reference_](https://solana.com/docs/rpc/http/getaccountinfo)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
