---
description: Send your first Solana account read with JSON-RPC.
---

# Quickstart

In this quickstart we read account state two ways, on the same task:

1. **Standard Agave polling.** You call an RPC node and it answers over the network, one request per read.
2. **Account Sync (`@triton-one/triton-sdk`).** Keeps a local copy of the accounts you track in RAM and resolves reads against it, with no network round-trip. By default it auto-subscribes to each account the first time your code reads it, so the accounts you poll get streamed automatically (you can also pre-seed them with `initialAccounts`). It serves `getAccountInfo`, `getMultipleAccountsInfo`, `getParsedAccountInfo`, and `getMultipleParsedAccounts`, and it's a drop-in for web3.js with no code rewrite.

## Step 0. Prerequisites

* An active Triton subscription
* Your endpoint URL and secret token (for backends) from the [customer dashboard](https://customers.triton.one/)
* An environment in TypeScript, Rust, or curl

## Step 1. Install

{% tabs %}
{% tab title="Standard RPC" %}
{% tabs %}
{% tab title="TypeScript" %}
```bash
mkdir solana-reads && cd solana-reads
npm init -y
npm install @solana/web3.js
npm install --save-dev typescript ts-node @types/node
npx tsc --init
```
{% endtab %}

{% tab title="Rust (Cargo.toml)" %}
```toml
[dependencies]
solana-client = "2"
solana-pubkey = "2"
anyhow = "1"
```
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="Account Sync" %}
```bash
mkdir account-sync && cd account-sync
npm init -y
npm install @triton-one/triton-sdk
npm install --save-dev typescript ts-node @types/node
npx tsc --init
```
{% endtab %}
{% endtabs %}

## Step 2. Connect and read

**What we're doing:** read a single account, the USDC mint, and decode it with `jsonParsed`.

{% tabs %}
{% tab title="Standard RPC" %}
{% tabs %}
{% tab title="TypeScript" %}
```typescript
import { Connection, PublicKey } from "@solana/web3.js";

const connection = new Connection(
  "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>"
);

const usdcMint = new PublicKey("EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v");
const account = await connection.getParsedAccountInfo(usdcMint);

console.log(account.value);
```
{% endtab %}

{% tab title="curl" %}
```bash
curl https://<your-endpoint>.mainnet.rpcpool.com/<your-token> -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getAccountInfo",
    "params": [
      "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
      { "encoding": "jsonParsed" }
    ]
  }'
```
{% endtab %}

{% tab title="Rust" %}
```rust
use {solana_client::rpc_client::RpcClient, solana_pubkey::Pubkey, std::str::FromStr};

fn main() -> anyhow::Result<()> {
    let client = RpcClient::new(
        "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>".to_string(),
    );
    let usdc = Pubkey::from_str("EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v")?;
    let account = client.get_account(&usdc)?;
    println!("{:?}", account);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

For every read method and its parameters, see the [API reference](https://kate-6.gitbook.io/triton-one-docs-v5/api-reference).
{% endtab %}

{% tab title="Account Sync" %}
Swap `@solana/web3.js` for `@triton-one/triton-sdk`. Your polling code stays the same: the same `getParsedAccountInfo` call now resolves from a local, stream-fed cache instead of a network round-trip.

```typescript
import { Connection, PublicKey } from "@triton-one/triton-sdk";

const usdcMint = "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v";

const connection = new Connection(
  "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>",
  {
    accountSync: {
      transport: "grpc",           // Node backend; use "ws" in the browser
      initialAccounts: [usdcMint], // seed the accounts to stream and track
    },
  }
);

const account = await connection.getParsedAccountInfo(new PublicKey(usdcMint));
console.log(account.value);

await connection.close();
```

`transport: "grpc"` is the most efficient stream for Node backends; browsers must use `transport: "ws"`. Set `subscriptionEndpoint` only when your stream endpoint differs from your RPC endpoint; on Triton it's usually the same.
{% endtab %}
{% endtabs %}

## Step 3. Verify your read

Run your code:

{% tabs %}
{% tab title="TypeScript" %}
```bash
npx ts-node main.ts
```
{% endtab %}

{% tab title="Rust" %}
```bash
cargo run
```
{% endtab %}

{% tab title="curl" %}
The response prints straight to your terminal.
{% endtab %}
{% endtabs %}

Then check:

1. **You got a `result`, not an `error`.** A JSON-RPC error has an `error` object with a `code`. The common ones are in the FAQs below.
2. **The account exists.** `getAccountInfo` returns `null` when the address holds no account. The USDC mint above always returns data.

### Expected response

The USDC mint, decoded with `jsonParsed`. Over curl this is the `result.value`; through Account Sync, `getParsedAccountInfo(...).value` returns the same object. Live values like `supply` and `lamports` will differ when you run it.

```json
{
  "data": {
    "parsed": {
      "info": {
        "decimals": 6,
        "freezeAuthority": "7dGbd2QZcCKcTndnHcTL8q7SMVXAkp688NTQYwrRCrar",
        "isInitialized": true,
        "mintAuthority": "BJE5MMbqXjVwjAF7oxwPYXnTXDyspzZyt4vwenNw5ruG",
        "supply": "7957814436632629"
      },
      "type": "mint"
    },
    "program": "spl-token",
    "space": 82
  },
  "executable": false,
  "lamports": 511210011619,
  "owner": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
  "rentEpoch": 18446744073709551615,
  "space": 82
}
```

## FAQs

<details>

<summary>I'm getting 401 or 403 errors</summary>

Check your endpoint URL and secret token in the [customer dashboard](https://customers.triton.one). For a backend calling JSON-RPC, the token is part of the URL path: `https://<your-endpoint>.mainnet.rpcpool.com/<your-token>`. For a frontend, confirm your configured allowed origin matches where you're sending the request from.

</details>

<details>

<summary>Which should I use?</summary>

Use **standard Agave RPC polling** for infrequent or one-off reads. Use **Account Sync** when your app reads the same accounts repeatedly and needs fresh data at the lowest latency and cost. Responses on supported methods are identical, so you can drop Account Sync in instead of web3.js with no code rewrite.

</details>

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-rotate">:rotate:</i> <strong>Account Sync</strong></td><td>The web3.js-compatible SDK that keeps account reads fresh from a live stream.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/account-sync">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/account-sync</a></td></tr><tr><td><i class="fa-bolt">:bolt:</i> <strong>Cloudbreak custom indexes</strong></td><td>Fast account and token queries from tailored dynamic indexes.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/cloudbreak-indexed-accounts">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/cloudbreak-indexed-accounts</a></td></tr><tr><td><i class="fa-gem">:gem:</i> <strong>Metaplex DAS API</strong></td><td>Query NFTs, tokens, and compressed assets via the Metaplex Digital Asset Standard interface.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/metaplex-das-api">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/metaplex-das-api</a></td></tr><tr><td><i class="fa-radio">:radio:</i> <strong>Real-time streaming</strong></td><td>Subscribe to account and transaction changes as they happen on chain.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
