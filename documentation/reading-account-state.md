---
description: How to read Solana account state on Triton, and how to choose between standard RPC, Cloudbreak, Account Sync, and the DAS API.
layout:
  pagination:
    visible: false
---

# Reading account state

Triton offers several ways to read Solana account state. This page covers how account reads work, what each option does, and how to pick the right one for your build.

## How account reads work

Solana state lives in accounts. The standard way to read it is JSON-RPC: `getAccountInfo` and `getMultipleAccounts` for specific accounts, `getProgramAccounts` for every account a program owns, and the SPL token methods (`getTokenAccountsByOwner` and so on). These work on every Triton endpoint and need no special setup.

Two patterns get expensive as you scale:

* **`getProgramAccounts` and token queries** scan large account sets. As a program grows toward millions of accounts, an unindexed scan gets slow and can time out.
* **Polling for fresh state.** Calling `getAccountInfo` on a loop to keep data current means your reads are stale between calls, and you burn requests (and hit rate limits) hammering the same endpoint.

Triton's account-read products each target one of these, while keeping the standard Solana JSON-RPC shape so your client code barely changes.

## Pick your read path

| Capability                                  |  Standard RPC   |    Cloudbreak    |          Account Sync          |    DAS API    |
| ------------------------------------------- | :-------------: | :--------------: | :----------------------------: | :-----------: |
| `getAccountInfo` / `getMultipleAccounts`    |        ✓        |         ✓        |                ✓               |       ✗       |
| `getProgramAccounts` at scale               |      scans      |    ✓ (indexed)   |                ✗               |       ✗       |
| SPL token queries (owner / mint / delegate) |        ✓        |    ✓ (indexed)   |                ✗               |       ✗       |
| NFT, cNFT, and token metadata               |        ✗        |         ✗        |                ✗               |       ✓       |
| Live-synced reads (no polling)              |        ✗        |         ✗        |                ✓               |       ✗       |
| Interface                                   | Solana JSON-RPC |  Solana JSON-RPC | Triton SDK (drop-in web3.js)   | DAS JSON-RPC  |
| Client changes                              |       none      |       none       |       one-line import swap      |  DAS methods  |

* **Standard RPC** is the baseline. Every Triton endpoint serves the full Solana JSON-RPC account methods. Use it when account sets are small and per-call freshness is fine.
* **Cloudbreak** answers `getProgramAccounts`, account lookups, and SPL token queries from a purpose-built index, so they stay fast even for programs with millions of accounts. It serves the same standard methods, so there is nothing to change on your side. It is the Accounts module of RPC 2.0.
* **Account Sync** keeps a live account stream open and serves `getAccountInfo` and `getMultipleAccounts` from an in-memory buffer that the stream keeps fresh, so you stop polling. It ships as a drop-in `@solana/web3.js` replacement, so adopting it is a one-line import swap.
* **DAS API** reads digital assets, regular and compressed NFTs, and SPL and Token-2022 tokens, via the Metaplex Digital Asset Standard.

It is normal to combine them. Common patterns:

* **App reading many program accounts (DEX, lending):** Cloudbreak for indexed `getProgramAccounts`.
* **Frontend or bot tracking a known set of accounts live:** Account Sync, no polling.
* **Wallet or NFT app:** DAS API for assets, Cloudbreak or Account Sync for balances.

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-database">:database:</i> <strong>Cloudbreak</strong></td><td>Indexed account reads. Fast getProgramAccounts and SPL token queries.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/cloudbreak-indexed-accounts">Cloudbreak</a></td></tr><tr><td><i class="fa-arrows-rotate">:arrows-rotate:</i> <strong>Account Sync</strong></td><td>Streaming-backed local cache for account reads. No polling, one-line SDK swap.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/account-sync">Account Sync</a></td></tr><tr><td><i class="fa-image">:image:</i> <strong>DAS API</strong></td><td>Read NFTs, cNFTs, and tokens via the Metaplex Digital Asset Standard.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/metaplex-das-api">DAS API</a></td></tr></tbody></table>

## Compressed accounts

Light Protocol (ZK Compression) accounts are not served by `getProgramAccounts` or the streams. Read them with the Triton-hosted Photon service, which queries individual compressed accounts, token balances, validity proofs, and transaction signatures directly.

## Pricing

Standard RPC and Cloudbreak account reads are billed at `$0.08 / GB` of bandwidth plus `$10 / million` requests. DAS API requests are `$0.08 / GB` plus `$50 / million`. Account Sync streams under the hood, so it is billed on bandwidth at `$0.08 / GB`.

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-play">:play:</i> <strong>Quickstart</strong></td><td>Read account state with standard RPC and Account Sync in a few minutes.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/quickstart">Quickstart</a></td></tr><tr><td><i class="fa-list-check">:list-check:</i> <strong>Best practices</strong></td><td>Get the most out of Triton's account-read products.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/best-practices">Best practices</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
