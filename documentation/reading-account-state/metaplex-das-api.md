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

The DAS API provides a fast and scalable way to query digital assets on Solana, including fungible tokens and regular and compressed NFTs. Decompression and data caching are done server-side, a convenient way to access digital asset data and metadata. It is available on all our shared and dedicated endpoints, fully integrated with our RPC Pool infrastructure.

Over two years we've delivered major open-source enhancements to DAS: ingestion re-architected onto Yellowstone gRPC, new parsers for fungible tokens and Token-2022, and a full query rework that brings `getAsset` to ~50 ms at P90.

## Features

* **Transaction history of any compressed asset.**
* **Current token holders and balances.**
* **Search and filtering by owner, creator, authority, or collection.**
* **Data across regular and programmable NFTs, compressed NFTs, MPL Core, fungible tokens, and Metaplex agent tokens.**
* **Merkle proofs for compressed assets**, with server-side decompression: an asset and its metadata return in a single call.

## How to use

DAS is enabled by default on every Triton endpoint.

### Methods

Every method's parameters and example request and response are in the [DAS API reference](/api-reference/solana/reading-account-state/das-api). Grouped by what they do:

* **Read assets:** `getAsset`, `getAssets`, `getAssetBatch`, `getAssetProof`, `getAssetProofBatch`
* **List assets:** `getAssetsByOwner`, `getAssetsByAuthority`, `getAssetsByCreator`, `getAssetsByGroup`, `searchAssets`
* **Tokens, editions, and history:** `getTokenAccounts`, `getTokenLargestAccounts`, `getNftEditions`, `getSignaturesForAsset`

`getAssetsByGroup` is temporarily disabled for performance work.

### Example: fetch an asset

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

### Pricing and limits

DAS is billed at `$0.08 / GB` of bandwidth plus `$50 / million` requests. Standard RPC Pool rate limits apply. For detailed billing, see the Billable Items Chart in your [customer dashboard](https://customers.triton.one).

## Further reading

Metaplex publishes a JavaScript DAS client with examples: [digital-asset-standard-api](https://github.com/metaplex-foundation/digital-asset-standard-api/tree/main/clients/js#examples). For the protocol and asset model, see [developers.metaplex.com/das-api](https://developers.metaplex.com/das-api).

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>DAS API reference</strong></td><td>Parameters and example request and response for every DAS method.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/api-reference/solana/reading-account-state/das-api">DAS API reference</a></td></tr><tr><td><strong>Best practices for reading account state</strong></td><td>When to reach for DAS versus raw account reads, and how to keep queries cheap.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/best-practices">Best practices for reading account state</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
