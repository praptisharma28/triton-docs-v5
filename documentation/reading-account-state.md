---
description: Standard RPC, Cloudbreak, Account Sync, and the DAS API overview.
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

# Reading account state

Solana keeps all of its state in accounts: a wallet, each token balance, a DEX market, an open order, and the program code itself. The standard way to read this state is Solana JSON-RPC polling, supported by every Triton endpoint.

However, Agave nodes make some of these reads slow or expensive as you scale:

* **Set queries like gPA.** Fetching every account a program owns scans the program's entire account set and applies filters only after each account is loaded, so an unindexed scan gets slow and can time out on large programs.
* **Polling for fresh state.** Re-fetching the same accounts on a loop leaves your data stale between calls and burns requests (and rate limits) against the same endpoint.
* **NFT and compressed NFT.** Assembling an asset's owner, metadata, and (for compressed NFTs) its Merkle proof from raw accounts takes custom indexing, decompression, and many calls.

Triton's stack targets each of these, so you read accounts state fast and cost-efficiently while keeping the standard Solana JSON-RPC response shape:

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-server">:server:</i> <strong>Standard RPC</strong></td><td>Every standard Solana JSON-RPC account method is supported. Most route through specialized pipelines for faster, cheaper responses, while the rest rely on standard Agave nodes.</td><td><a href="https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana">https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana</a></td></tr><tr><td><i class="fa-database">:database:</i> <strong>Cloudbreak</strong></td><td>Tailored indexes built from your live traffic for 99% faster account and token reads. Your requests are indexed and routed through Cloudbreak automatically, with nothing for you to set up.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/cloudbreak-indexed-accounts">https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/cloudbreak-indexed-accounts</a></td></tr><tr><td><i class="fa-arrows-rotate">:arrows-rotate:</i> <strong>Account Sync</strong></td><td>Account reads served from a local cache kept fresh by a live gRPC or WS stream. You get streaming-grade speed and bandwidth-only billing for your existing polling code.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/account-sync">https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/account-sync</a></td></tr><tr><td><i class="fa-image">:image:</i> <strong>DAS API</strong></td><td>One API to fetch ownership, metadata, and balances for NFTs, cNFTs, and tokens, via the Metaplex Digital Asset Standard, an extension of JSON-RPC.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/metaplex-das-api">https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/metaplex-das-api</a></td></tr></tbody></table>

### Read path for each method

Every account-read method is served by one of these. Standard RPC covers the methods the node answers directly; the rest route through a specialized service.

| Service                                                                             | Account-read methods                                                                                                                                                                                                                             |
| ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Standard RPC** (Agave node polling, unless using Account Sync)                    | `getTokenLargestAccounts`, `getTokenSupply`, `getVoteAccounts`, `getStakeMinimumDelegation`, `getMinimumBalanceForRentExemption`                                                                                                                 |
| **Cloudbreak** (your requests route through it by default, no changes on your side) | `getProgramAccounts`, `getAccountInfo`, `getMultipleAccounts`, `getTokenAccountsByOwner`, `getTokenAccountsByDelegate`, `getSlot`, `getTokenAccountsByMint`, `getBalance`, `getTokenAccountBalance`, `getVersion`, `getGenesisHash`, `getHealth` |
| **Account Sync** (served from RAM via SDK)                                          | `getAccountInfo`, `getMultipleAccountsInfo`, `getParsedAccountInfo`, `getMultipleParsedAccounts` (and `*AndContext` variants)                                                                                                                    |
| **DAS API** (Metaplex JSON-RPC extension)                                           | `getAsset`, `getAssets`, `getAssetProof`, `getAssetsByOwner`, `getAssetsByAuthority`, `getAssetsByCreator`, `getAssetsByGroup`, `searchAssets`, `getTokenAccounts`, `getNftEditions`, `getSignaturesForAsset`                                    |

The single and multiple account reads (`getAccountInfo`, `getMultipleAccounts`) route through Cloudbreak by default, but you can also use Account Sync (the `@triton-one/triton-sdk` SDK) to resolve them from your local cache, faster and cheaper.

## Use cases

* **Reading many program accounts at scale** (DEX, lending, indexers): Cloudbreak serves indexed `getProgramAccounts` and token queries 500x+ faster on repeated filter shapes.
* **Tracking a set of accounts live** (any frontend or backend with a large polling codebase): Account Sync keeps your existing code, stores it locally in RAM, backed by a stream, and serves your reads from there for lower latency and cost.
* **Wallets and NFT apps**: DAS API fetches assets and metadata you need in one call, while Cloudbreak or Account Sync handle user balances lookups.
* **Low-latency trading and market making**: to get the fastest data, we recommend streaming it with gRPC ([Dragon's Mouth](https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/dragon-s-mouth-grpc)). If you want account reads in a web3.js-shaped client, Account Sync cuts read latency significantly compared to polling.

## Limitations

* **Excluded methods.** `getLargestAccounts` is not served on shared endpoints. `getTokenLargestAccounts` does not accept JSON-RPC batch requests.
* **Light Protocol (ZK Compression) accounts** are not served by `getProgramAccounts` or the Yellowstone streams.
* **Commitment.** Cloudbreak ingests from a gRPC stream at `confirmed` commitment, so `processed` is not served on the methods routed through Cloudbreak.

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-play">:play:</i> <strong>Quickstart</strong></td><td>Query account state via JSON-RPC and set up Account Sync in under 2 minutes.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/quickstart">https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/quickstart</a></td></tr><tr><td><i class="fa-list-check">:list-check:</i> <strong>Best practices</strong></td><td>How to reach the lowest latency, maximum performance, and minimum cost on your account state reads.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/best-practices">https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/best-practices</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
