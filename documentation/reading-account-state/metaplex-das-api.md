---
description: Query Solana NFTs, compressed NFTs, and fungible tokens through the Metaplex Digital Asset Standard API.
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

# Metaplex DAS API

The Digital Asset Standard (DAS) API is the Metaplex read API for Solana assets: regular NFTs, compressed NFTs, and fungible tokens (SPL and Token-2022). Triton co-develops DAS and serves it with server-side decompression and a caching layer, so a compressed NFT and its metadata come back in a single call.

DAS is enabled by default on every Triton shared and dedicated Solana endpoint. Calls to the DAS methods route automatically, with no separate setup.

## Use DAS when

You want assets by owner, creator, authority, or group; Merkle proofs for compressed NFTs; or a fungible token's balance and mint metadata, without running `getProgramAccounts` scans or decompressing cNFTs yourself.

## Methods

Every method's parameters and example request and response are in the [DAS API reference](https://kate-6.gitbook.io/triton-one-docs-v5/api-reference/solana/reading-account-state/das-api). Grouped by what they do:

* **Read assets:** `getAsset`, `getAssets`, `getAssetBatch`, `getAssetProof`, `getAssetProofBatch`
* **List assets:** `getAssetsByOwner`, `getAssetsByAuthority`, `getAssetsByCreator`, `getAssetsByGroup`, `searchAssets`
* **Tokens, editions, and history:** `getTokenAccounts`, `getTokenLargestAccounts`, `getNftEditions`, `getSignaturesForAsset`

`getAssetsByGroup` is temporarily disabled for performance work.

## Example: fetch an asset

`getAsset` returns one asset by ID. The example uses the USDC mint, so the response carries fungible `token_info` (supply, decimals, token program):

```bash
curl https://<your-endpoint>.mainnet.rpcpool.com/<your-token> \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getAsset",
    "params": { "id": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v" }
  }'
```

## Clients and SDKs

Metaplex publishes a JavaScript DAS client with examples: [digital-asset-standard-api](https://github.com/metaplex-foundation/digital-asset-standard-api/tree/main/clients/js#examples). For the protocol and asset model, see [developers.metaplex.com/das-api](https://developers.metaplex.com/das-api).

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>DAS API reference</strong></td><td>Parameters and example request and response for every DAS method.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/api-reference/solana/reading-account-state/das-api">DAS API reference</a></td></tr><tr><td><strong>Reading account state best practices</strong></td><td>When to reach for DAS versus raw account reads, and how to keep queries cheap.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/best-practices">Reading account state best practices</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
