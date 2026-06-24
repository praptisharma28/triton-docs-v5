---
description: "Standard RPC, Cloudbreak, Account Sync, and the DAS API: a services overview."
layout:
  pagination:
    visible: false
---

# Reading account state

## How account reads work

Everything on Solana is an account. The chain has two primitives: a transaction records an event, and an account holds the current state of everything else: a wallet, each token balance, a DEX market, an order, and the program code itself. Every account is owned by a program that can change its data, and the standard way to read that state is Solana JSON-RPC, which every Triton endpoint serves.

Two read patterns get expensive as you scale:

* **Set queries.** Fetching every account a program owns scans a large set. Solana's account store is built for single-account lookups during execution, not set queries, so an unindexed scan slows down and can time out as a program grows toward millions of accounts.
* **Polling for fresh state.** Re-fetching the same accounts on a loop leaves your data stale between calls and burns requests (and rate limits) against the same endpoint.

Each Triton account-read product targets one of these while keeping the standard Solana JSON-RPC response shape, so responses stay compatible and your client code changes little to nothing.

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-server">:server:</i> <strong>Standard RPC</strong></td><td>Every standard Solana JSON-RPC account method. Most route through specialized pipelines for faster, cheaper responses; the rest stay standard JSON-RPC. Nothing to set up.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/api-reference/solana">https://kate-6.gitbook.io/triton-one-docs-v5/api-reference/solana</a></td></tr><tr><td><i class="fa-database">:database:</i> <strong>Cloudbreak</strong></td><td>Indexed account and token reads, up to 99% faster with tailored dynamic indexes. Your requests route through Cloudbreak automatically, with no changes on your side.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/cloudbreak-indexed-accounts">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/cloudbreak-indexed-accounts</a></td></tr><tr><td><i class="fa-arrows-rotate">:arrows-rotate:</i> <strong>Account Sync</strong></td><td>Account reads served from a local cache kept fresh by a live stream: no polling, billed on bandwidth only. Adopt it with a one-line SDK swap.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/account-sync">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/account-sync</a></td></tr><tr><td><i class="fa-image">:image:</i> <strong>DAS API</strong></td><td>One API for NFTs, cNFTs, and tokens via the Metaplex Digital Asset Standard. Call it through the DAS JSON-RPC extension.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/metaplex-das-api">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/metaplex-das-api</a></td></tr></tbody></table>

## Methods by service

Every account-read method is served by one of these. Standard RPC covers the methods the node answers directly; the rest route through a specialized service.

| Service | Account-read methods |
| --- | --- |
| **Standard RPC** (standard Agave node) | `getTokenLargestAccounts` |
| **Cloudbreak** (your requests route through it by default, no changes on your side) | `getProgramAccounts`, `getAccountInfo`, `getMultipleAccounts`, `getTokenAccountsByOwner`, `getTokenAccountsByDelegate`, `getSlot`, `getTokenAccountsByMint`, `getBalance`, `getTokenAccountBalance`, `getVersion`, `getGenesisHash`, `getHealth` |
| **Account Sync** (served from RAM via SDK) | `getAccountInfo`, `getMultipleAccountsInfo`, `getParsedAccountInfo`, `getMultipleParsedAccounts` (and `*AndContext` variants) |
| **DAS API** (Metaplex JSON-RPC extension) | `getAsset`, `getAssets`, `getAssetProof`, `getAssetsByOwner`, `getAssetsByAuthority`, `getAssetsByCreator`, `getAssetsByGroup`, `searchAssets`, `getTokenAccounts`, `getNftEditions`, `getSignaturesForAsset` |

The single and multiple account reads (`getAccountInfo`, `getMultipleAccounts`) route through Cloudbreak by default and can also be served from the Account Sync cache: same methods, different access pattern.

## Use cases

* **Reading many program accounts at scale** (DEX, lending, indexers): Cloudbreak serves indexed `getProgramAccounts` and token queries fast when your filter shapes repeat.
* **Tracking a known set of accounts live** (frontends, bots): Account Sync keeps them fresh from a stream, so you stop polling.
* **Wallets and NFT apps**: the DAS API for assets and metadata, Cloudbreak or Account Sync for balances.
* **Low-latency trading and market making**: for the lowest latency, stream with gRPC ([Dragon's Mouth](/documentation/solana/real-time-streaming/dragon-s-mouth-grpc)). When you need account reads in a web3.js-shaped client, Account Sync cuts read latency significantly versus polling.

## Limitations and exclusions

* **Excluded methods.** `getLargestAccounts` is not served on shared endpoints.
* **Light Protocol (ZK Compression) accounts** are not served by `getProgramAccounts` or the streams.
* **Cloudbreak indexes a configured set of programs.** A `getProgramAccounts` or token query for a program outside that set returns `-32010` (`KEY_EXCLUDED_FROM_SECONDARY_INDEX`), and account lookups return `null` (or `0` for a balance) for an account not in the indexed set.
* **Methods Cloudbreak does not implement** (`getVoteAccounts`, transaction methods, block methods) are served by the standard node as usual, not accelerated.

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-play">:play:</i> <strong>Quickstart</strong></td><td>Read account state with standard RPC and Account Sync in a few minutes.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/quickstart">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/quickstart</a></td></tr><tr><td><i class="fa-list-check">:list-check:</i> <strong>Best practices</strong></td><td>When to reach for Cloudbreak, Account Sync, or the DAS API, how to keep getProgramAccounts cheap with repeatable filter shapes, and how to size Account Sync's tracked set and timeouts.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/best-practices">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/best-practices</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
