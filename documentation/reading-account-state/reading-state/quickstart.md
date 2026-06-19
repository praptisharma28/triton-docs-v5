# Quickstart

Read Solana account state in five minutes, two ways: the standard JSON-RPC reads, and Account Sync, a web3.js drop-in that serves reads from a live stream. For the overview and a product comparison, see the [Reading account state overview](../../reading-account-state.md).

## Step 0. Prerequisites

Both methods run on your Triton endpoint with token-based auth. Before starting, here's what you need:

* An active Triton subscription
* Your endpoint URL and secret token from the [customer dashboard](https://customers.triton.one/)
* A backend in TypeScript, Rust, or Python (Account Sync is a TypeScript SDK)
* You've picked a method to try

The two ways to read covered here:

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-server">:server:</i> <strong>Standard RPC</strong></td><td>The standard Solana JSON-RPC reads: getAccountInfo, getMultipleAccounts, getBalance.</td><td></td></tr><tr><td><i class="fa-rotate">:rotate:</i> <strong>Account Sync</strong></td><td>A web3.js drop-in that serves account reads from a live stream instead of a round-trip.</td><td></td></tr></tbody></table>

## Step 1. Install

{% tabs %}
{% tab title="Standard RPC" %}
The reads are plain JSON-RPC, so `curl` needs nothing. For a typed client:

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

The SDK re-exports web3.js types, so most apps import from this one package.
{% endtab %}
{% endtabs %}

## Step 2. Connect and read

{% tabs %}
{% tab title="Standard RPC" %}
**What we're doing:** read a single account, the USDC mint, and decode it with `jsonParsed`.

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
**What we're doing:** swap `@solana/web3.js` for `@triton-one/triton-sdk` so `getAccountInfo` serves from a live in-memory stream instead of a fresh RPC round-trip. The code still looks like web3.js.

```typescript
import { Connection, PublicKey } from "@triton-one/triton-sdk";

const accountId = "ping6gwBZx1ccMMFyLgkVSupUmujYrFidEXuNRPq989";

const connection = new Connection(
  "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>",
  {
    accountSync: {
      transport: "grpc", // use "ws" in the browser
      initialAccounts: [accountId],
    },
  }
);

const account = await connection.getAccountInfo(new PublicKey(accountId));
console.log(account);

await connection.close();
```

Migrating an existing app is usually one import change plus the `accountSync` config: keep calling `getAccountInfo` and `getMultipleAccountsInfo` as before, and call `close()` when you're done.
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

### Expected first response

{% tabs %}
{% tab title="Standard RPC" %}
The USDC mint decoded with `jsonParsed`: a `result.value` with the account's `lamports`, `owner` (the SPL Token program), and a `data.parsed` object describing the mint (decimals, supply, mint and freeze authorities).
{% endtab %}

{% tab title="Account Sync" %}
A standard web3.js `AccountInfo` for the tracked account (`lamports`, `owner`, `data`, `executable`, `rentEpoch`), served from the local stream-fed buffer rather than a fresh RPC call.
{% endtab %}
{% endtabs %}

## FAQs

<details>

<summary>I'm getting 401 or 403 errors</summary>

Check your endpoint URL and secret token in the [customer dashboard](https://customers.triton.one). For HTTP RPC the token is part of the URL path: `https://<your-endpoint>.mainnet.rpcpool.com/<your-token>`. If you've configured allowed origins, confirm your origin matches what's whitelisted.

</details>

<details>

<summary>Which transport should Account Sync use?</summary>

Use `transport: "grpc"` in Node backends for the most efficient stream. Browsers can only use `transport: "ws"`. Set `subscriptionEndpoint` only when your stream endpoint differs from your RPC endpoint; in most cases it's the same Triton endpoint.

</details>

<details>

<summary>Account Sync returns null for an account I expect to exist</summary>

Account Sync waits up to `missTimeoutMs` (default 5000 ms) for an untracked account's data before returning `null`, because an account you derived deterministically might not be created on-chain yet. Tune `missTimeoutMs`, or pre-subscribe with `initialAccounts` or `addAccounts` so the data is already streaming when you read.

</details>

<details>

<summary>Which should I use?</summary>

Use **Standard RPC** for one-off account and balance reads: it's a normal request and response, no streaming state to manage. Use **Account Sync** when your app reads the same accounts repeatedly and wants them kept fresh by a stream rather than re-fetched on every call. The API is web3.js-compatible, so most apps migrate with one import change.

</details>

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-rotate">:rotate:</i> <strong>Account Sync</strong></td><td>The web3.js-compatible SDK that keeps account reads fresh from a live stream.</td><td></td></tr><tr><td><i class="fa-bolt">:bolt:</i> <strong>Cloudbreak custom indexes</strong></td><td>Fast getProgramAccounts and SPL token queries from a purpose-built index.</td><td></td></tr><tr><td><i class="fa-gem">:gem:</i> <strong>Metaplex DAS API</strong></td><td>Query NFTs, tokens, and compressed assets across shared and dedicated endpoints.</td><td></td></tr><tr><td><i class="fa-radio">:radio:</i> <strong>Real-time streaming</strong></td><td>Subscribe to account and transaction changes as they happen with Dragon's Mouth.</td><td></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
