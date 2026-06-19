---
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

The Digital Asset Standard (DAS) API is Metaplex's read API for NFTs, compressed NFTs (Bubblegum), and fungible tokens. Fetch one asset with `getAsset` or many at once with `getAssets` (`getAssetBatch` is an alias), and list the assets held by a wallet or tied to an authority, creator, or collection with `getAssetsByOwner`, `getAssetsByAuthority`, `getAssetsByCreator`, and `getAssetsByGroup`. `searchAssets` filters across those dimensions in a single query.

For compressed assets, `getAssetProof` and `getAssetProofBatch` return the Merkle proof needed to verify or transfer them, and `getSignaturesForAsset` lists an asset's transaction history. `getTokenAccounts` and `getTokenLargestAccounts` cover fungible-token holders, and `getNftEditions` lists the prints of a master edition. Use the DAS API instead of running your own indexer for digital-asset reads, especially compressed NFTs, which standard RPC cannot resolve from account state alone.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
