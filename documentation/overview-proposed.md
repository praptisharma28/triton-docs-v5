---
description: Triton's Solana stack and the builder paths to get started.
---

# Overview

Triton has built on Solana since the testnet, as one of the network's original validators. Today we run premium, globally distributed bare-metal infrastructure for many of the ecosystem's most demanding teams across DeFi, trading, and institutions.

Alongside running infrastructure, we author core public tooling the ecosystem depends on. Our best-known work:

* **RPC 2.0:** a ground-up rebuild of Solana's read layer for faster, cheaper, more expressive account and ledger queries
* **Yellowstone gRPC (Dragon's Mouth):** Geyser-based gRPC streaming for account, transaction, slot, and block updates, now the de facto Solana streaming standard
* **Old Faithful:** the only complete, verified, public Solana archive, stored as content-addressable CAR files and queryable back to genesis
* **Metaplex DAS API:** we co-develop the Digital Asset Standard, the ecosystem's read API for NFTs, cNFTs, and tokens, and serve it with server-side decompression so a compressed NFT and its metadata come back in one call

## Triton stack

We offer a complete Solana stack for any use case: shared infrastructure, dedicated nodes, and validator and staking services.

### Shared infrastructure

Every shared cluster endpoint gives you the full product stack below, with flexible connection and rate limits. We have split Solana's read layer into purpose-built, independent components that scale automatically and absorb spikes with no tuning on your side. Combined with global distribution and instant auto-failover, shared is the right answer for most workloads.

{% tabs %}
{% tab title="Reading state" %}
<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-database">:database:</i> <strong>Cloudbreak</strong></td><td>Indexed account reads: getProgramAccounts and SPL token queries over 99% faster and 7x cheaper than standard RPC, even for programs with millions of accounts.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/cloudbreak-indexed-accounts">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/cloudbreak-indexed-accounts</a></td></tr><tr><td><i class="fa-image">:image:</i> <strong>DAS API</strong></td><td>One API for NFTs, cNFTs, and tokens via the Metaplex Digital Asset Standard, with server-side cNFT decompression.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/metaplex-das-api">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/metaplex-das-api</a></td></tr><tr><td><i class="fa-arrows-rotate">:arrows-rotate:</i> <strong>Account Sync</strong></td><td>One-line SDK swap that serves account reads from a local cache kept fresh by a live stream. Streaming-grade speed, billed on bandwidth only.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/account-sync">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/account-sync</a></td></tr></tbody></table>
{% endtab %}

{% tab title="Streaming" %}
<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-radio">:radio:</i> <strong>Dragon's Mouth gRPC</strong></td><td>Geyser-fed gRPC streaming. Sub-slot account, transaction, and block subscriptions.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/dragon-s-mouth-grpc">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/dragon-s-mouth-grpc</a></td></tr><tr><td><i class="fa-fire">:fire:</i> <strong>Deshred transactions</strong></td><td>Pre-execution feed from raw shreds: about 20 ms ahead of confirmed transactions at p75, before execution context.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/deshred-transactions">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/deshred-transactions</a></td></tr><tr><td><i class="fa-rotate-right">:rotate-right:</i> <strong>Whirligig</strong></td><td>A gRPC-powered Solana WebSocket layer for standard WebSocket clients.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/whirligig-websockets">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/whirligig-websockets</a></td></tr><tr><td><i class="fa-layer-group">:layer-group:</i> <strong>Fumarole</strong></td><td>Persistent gRPC streams: a 4-day buffer and resume from your last slot on disconnect.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/fumarole-persistent-streams">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/fumarole-persistent-streams</a></td></tr></tbody></table>
{% endtab %}

{% tab title="Sending txs" %}
<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-paper-plane">:paper-plane:</i> <strong>Jet sender</strong></td><td>TPU client with stake-weighted QoS and direct-to-leader routing over QUIC.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/jet-sender">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/jet-sender</a></td></tr><tr><td><i class="fa-arrow-trend-up">:arrow-trend-up:</i> <strong>Priority Fees API</strong></td><td>Per-program priority-fee estimates tuned to land under congestion.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/priority-fees-api">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/priority-fees-api</a></td></tr><tr><td><i class="fa-code-branch">:code-branch:</i> <strong>Metis</strong></td><td>Self-hosted Jupiter swap API, no staked JUP required.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/3rd-party-apis/metis-swap-api">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/3rd-party-apis/metis-swap-api</a></td></tr><tr><td><i class="fa-route">:route:</i> <strong>Titan Prime</strong></td><td>Streaming swap quotes and routes that re-optimise live as the market moves.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/3rd-party-apis/titan-swap-api">Titan Prime</a></td></tr><tr><td><i class="fa-box">:box:</i> <strong>Jito Bundles</strong></td><td>Submit and simulate Jito bundles on every Triton plan.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/3rd-party-apis/jito-bundles">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/3rd-party-apis/jito-bundles</a></td></tr></tbody></table>
{% endtab %}
{% endtabs %}

### Dedicated nodes

A single-tenant Solana node with isolated capacity, no noisy neighbours, no rate limits, and your choice of regions for easy colocation. It fits high-throughput workloads that need full bandwidth, dedicated compute, flat streaming costs, and the lowest, steadiest backend latency. Yellowstone gRPC is included.

gRPC streaming nodes are the most common dedicated setup. For a more demanding workload like indexing, we can also run a managed node for you: [contact us](https://triton.one/contact). See [dedicated nodes](/documentation/solana/dedicated-nodes).

### Validator and staking

We have run Solana validators since the testnet, and offer two staking paths. For treasuries, institutions, and custodians, run a white-label validator under your own brand and keys, with no operational overhead, and keep your own delegators. For smaller stake, delegate to our private trusted validator (from 2,500 SOL) and receive your staking rewards, block rewards, and Jito MEV paid weekly in USDC. See [validator services](/documentation/solana/validator-services).

## Common builds

Not sure which products you need? Pick the path that matches what you are building. Each guide wires up the full recommended stack for that use case, end to end. We are adding more over time.

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-arrow-trend-up">:arrow-trend-up:</i> <strong>Trading and market making</strong></td><td>Real-time DEX data, earliest signal, priority fees, and reliable transaction landing.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/guides/solana/readme/trading-and-market-making">https://kate-6.gitbook.io/triton-one-docs-v5/guides/solana/readme/trading-and-market-making</a></td></tr><tr><td><i class="fa-wallet">:wallet:</i> <strong>Wallet or consumer app</strong></td><td>Balances, history, NFT portfolio, and live updates for wallets and consumer apps.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/guides/solana/readme/wallet-or-consumer-app">https://kate-6.gitbook.io/triton-one-docs-v5/guides/solana/readme/wallet-or-consumer-app</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
