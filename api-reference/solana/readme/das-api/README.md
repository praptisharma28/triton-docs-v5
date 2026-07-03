---
description: Query NFTs, compressed assets, and token accounts through the Metaplex DAS API.
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

These fetch assets directly or by who owns, controls, or created them:

| Method | What it returns |
| ------ | --------------- |
| `getAsset` | Detailed information about one digital asset. |
| `getAssets` | The same, for many assets by ID (getAssetBatch is an alias). |
| `getAssetsByOwner` | The assets owned by an address. |
| `getAssetsByAuthority` | The assets under a given authority. |
| `getAssetsByCreator` | The assets created by a given creator. |
| `getAssetsByGroup` | The assets in a group, such as a collection. |
| `searchAssets` | The assets matching a set of filters. |

These verify and trace compressed assets:

| Method | What it returns |
| ------ | --------------- |
| `getAssetProof` | The Merkle proof for a compressed asset. |
| `getAssetProofBatch` | The Merkle proofs for many compressed assets. |
| `getSignaturesForAsset` | The transaction history of a compressed asset. |

These read token accounts, holders, and editions for a mint:

| Method | What it returns |
| ------ | --------------- |
| `getTokenAccounts` | The token accounts for a mint or owner. |
| `getTokenLargestAccounts` | The 20 largest token accounts for a mint. |
| `getNftEditions` | The printed editions of a master edition NFT. |

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
