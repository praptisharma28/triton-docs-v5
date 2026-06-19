---
description: >-
  We contribute to the Metaplex Digital Asset Standard API for querying Solana
  NFTs, tokens, and compressed assets.
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

We actively contribute to the development of the Metaplex Digital Assets Standard API (DAS) that is used to interact with Solana NFTs, SPL (Solana Program Library) Tokens and Token-2022 (Token Extension Program), ensuring also production-ready indexing and caching layer.

DAS API provides a fast and scalable way to query digital assets on the Solana blockchain, including Fungible Tokens, Regular and Compressed NFTs. The decompression of NFTs and data caching is done server-side, providing a convenient way to access Digital Assets data and metadata. It is available via all our shared and dedicated endpoints, fully integrated with our RPC Pool infrastructure.

DAS API offers a comprehensive set of features for querying digital assets, including:

* a complete history of blocks and transactions,
* quick access to account data and past account balances,
* Merkle proofs for compressed assets,
* a consistent, easy-to-use interface.

## How to use

The DAS API is enabled by default on all our shared and dedicated endpoints. You can use any of our Solana mainnet RPC endpoints with the DAS API methods, and your queries will be routed automatically.

Available methods are listed and documented in the [API Methods](https://docs.triton.one/digital-assets-api/metaplex-digital-assets-api) section.

The DAS API is billed on a per-request basis ($50 per million requests). To see detailed billing information, check the Billable Items Chart in your account panel at the customers.triton.one.

[Ratelimits](https://docs.triton.one/rpc-pool/ratelimits) as per your standard RPC Pool will apply.

## Further reading

To learn more about account state compression and Digital Assets, see [developers.metaplex.com/das-api](https://developers.metaplex.com/das-api).

Client and examples: [https://github.com/metaplex-foundation/digital-asset-standard-api](https://github.com/metaplex-foundation/digital-asset-standard-api/tree/main/clients/js#examples)

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
