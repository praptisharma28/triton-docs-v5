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
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# Metaplex DAS API

The DAS API provides a fast and scalable way to query digital assets on Solana, including fungible tokens and regular and compressed NFTs. Decompression and data caching are done server-side, a convenient way to access digital asset data and metadata. It is available on all our shared and dedicated endpoints, fully integrated with our RPC Pool infrastructure.

Over the last two years we've delivered major open-source enhancements to DAS: ingestion re-architected onto Yellowstone gRPC, new parsers for fungible tokens and Token-2022, and a full query rework that brings `getAsset` to ~50 ms at P90.

## Features and benefits

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-clock-rotate-left">:clock-rotate-left:</i> <strong>Asset transaction history</strong></td><td>The full on-chain signature history for any asset, both compressed and standard.</td><td></td></tr><tr><td><i class="fa-wallet">:wallet:</i> <strong>Easy token data</strong></td><td>Current holders and balances for any token.</td><td></td></tr><tr><td><i class="fa-filter">:filter:</i> <strong>Search and filter</strong></td><td>Find assets by owner, creator, authority, or collection.</td><td></td></tr><tr><td><i class="fa-shapes">:shapes:</i> <strong>Every asset type</strong></td><td>One interface across regular and programmable NFTs, compressed NFTs, MPL Core, fungible tokens, and Metaplex agent tokens.</td><td></td></tr><tr><td><i class="fa-box-open">:box-open:</i> <strong>Server-side decompression</strong></td><td>Decompressed Merkle proofs and metadata for any asset in a single call.</td><td></td></tr></tbody></table>

## Supported methods

Every method's parameters and example request and response are in the [DAS API reference](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/readme/das-api). Grouped by what they do:

| Group | Methods |
| --- | --- |
| **Read assets** | `getAsset`, `getAssets`, `getAssetBatch`, `getAssetProof`, `getAssetProofBatch` |
| **List assets** | `getAssetsByOwner`, `getAssetsByAuthority`, `getAssetsByCreator`, `getAssetsByGroup`, `searchAssets` |
| **Tokens, editions, and history** | `getTokenAccounts`, `getTokenLargestAccounts`, `getNftEditions`, `getSignaturesForAsset` |

`getAssetsByGroup` is temporarily disabled for performance work.

## Get started

DAS is enabled by default on every Triton subscription. Just point DAS calls at your endpoint and you're good to go:

* **Backend:** `https://<your-endpoint>.<network>.rpcpool.com/<your-token>`, or pass your token as an `x-token` header.
* **Frontend:** `https://<your-endpoint>.<network>.rpcpool.com/`, with your domain set as an allowed origin in your [customer dashboard](https://customers.triton.one).

### Example request: fetch an asset

`getAsset` returns one asset by ID. The example uses a Mad Lads NFT, so the response carries the asset's metadata, owner, royalty, and collection:

```bash
curl https://<your-endpoint>.mainnet.rpcpool.com/<your-token> \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getAsset",
    "params": { "id": "F9Lw3ki3hJ7PF9HQXsBzoY8GyE6sPoEZZdXJBsTTD2rk" }
  }'
```

Metaplex publishes a JavaScript DAS client with examples: [digital-asset-standard-api](https://github.com/metaplex-foundation/digital-asset-standard-api/tree/main/clients/js#examples). For the protocol and asset model, see [developers.metaplex.com/das-api](https://developers.metaplex.com/das-api).

## Pricing

DAS is billed at `$0.08 / GB` of bandwidth plus `$50 / million` requests.

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>DAS API reference</strong></td><td>Parameters and example request and response for every DAS method.</td><td><a href="https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/readme/das-api">DAS API reference</a></td></tr><tr><td><i class="fa-list-check">:list-check:</i> <strong>Best practices</strong></td><td>How to reach the lowest latency, maximum performance, and minimum cost on your account state reads.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/best-practices">Best practices</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
