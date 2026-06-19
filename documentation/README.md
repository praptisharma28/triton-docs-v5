---
description: Triton's Solana stack and the builder paths to get started.
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

# Overview

Triton has built on Solana since the testnet, as one of the network's original validators. Today we run premium, globally distributed bare-metal infrastructure for many of the ecosystem's most demanding teams across DeFi, trading, and institutions.

Alongside running infrastructure, we author much of the core Solana tooling the ecosystem depends on. Our best-known work:

* **RPC 2.0:** a ground-up rebuild of Solana's read layer for faster, cheaper, more expressive queries ([more context](https://blog.triton.one/announcing-rpc-2-0-with-solana-foundation-rethinking-solanas-read-layer-from-the-ground-up/))
* **Yellowstone gRPC (Dragon's Mouth):** the Geyser-based streaming interface that became the Solana streaming standard
* **Old Faithful:** the only complete, verified, public Solana archive, queryable back to genesis
* **Metaplex DAS API:** core contributions to the ecosystem's standard read API for tokens and NFTs

## Triton stack

We run Solana infrastructure three ways: shared endpoints, dedicated nodes, and validator services.

### Shared

Every endpoint reaches the full product stack below. Solana's read layer has split into purpose-built components, so geographic distribution and per-component scaling are what drive both low latency and spike absorption. For most workloads, this is the right answer.

{% tabs %}
{% tab title="Reading state" %}
<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-database">:database:</i> <strong>Cloudbreak</strong></td><td>Indexed account reads for every app. Accounts module of RPC 2.0.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/cloudbreak-indexed-accounts">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/cloudbreak-indexed-accounts</a></td></tr><tr><td><i class="fa-image">:image:</i> <strong>DAS API</strong></td><td>Read NFTs, cNFTs, and tokens via the Metaplex Digital Asset Standard.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/metaplex-das-api">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/metaplex-das-api</a></td></tr><tr><td><i class="fa-arrows-rotate">:arrows-rotate:</i> <strong>Account Sync</strong></td><td>A live account-state mirror that replaces polling with a one-line SDK swap.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/account-sync">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/reading-account-state/account-sync</a></td></tr></tbody></table>
{% endtab %}

{% tab title="Streaming" %}
<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-radio">:radio:</i> <strong>Dragon's Mouth gRPC</strong></td><td>Geyser-fed gRPC streaming. Sub-slot account, transaction, and block subscriptions.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/dragon-s-mouth-grpc">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/dragon-s-mouth-grpc</a></td></tr><tr><td><i class="fa-fire">:fire:</i> <strong>Deshred transactions</strong></td><td>Transactions rebuilt from shreds pre-execution. The earliest on-chain signal.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/deshred-transactions">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/deshred-transactions</a></td></tr><tr><td><i class="fa-rotate-right">:rotate-right:</i> <strong>Whirligig</strong></td><td>A gRPC-powered Solana WebSocket layer for standard WebSocket clients.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/whirligig-websockets">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/whirligig-websockets</a></td></tr><tr><td><i class="fa-layer-group">:layer-group:</i> <strong>Fumarole</strong></td><td>Persistent gRPC streams with a 4-day buffer and resume on disconnect.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/fumarole-persistent-streams">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/real-time-streaming/fumarole-persistent-streams</a></td></tr></tbody></table>
{% endtab %}

{% tab title="Sending txs" %}
<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-paper-plane">:paper-plane:</i> <strong>Jet sender</strong></td><td>TPU client with stake-weighted QoS and direct-to-leader routing over QUIC.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/jet-sender">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/jet-sender</a></td></tr><tr><td><i class="fa-arrow-trend-up">:arrow-trend-up:</i> <strong>Priority Fees API</strong></td><td>Per-program priority-fee estimates tuned to land under congestion.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/priority-fees-api">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/priority-fees-api</a></td></tr><tr><td><i class="fa-code-branch">:code-branch:</i> <strong>Metis</strong></td><td>Self-hosted Jupiter swap API, no staked JUP required.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/3rd-party-apis/metis-swap-api">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/3rd-party-apis/metis-swap-api</a></td></tr><tr><td><i class="fa-route">:route:</i> <strong>Titan Prime</strong></td><td>Streaming swap quotes and routes that re-optimise live as the market moves.</td><td></td></tr><tr><td><i class="fa-box">:box:</i> <strong>Jito Bundles</strong></td><td>Submit and simulate Jito bundles on every Triton plan.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/3rd-party-apis/jito-bundles">https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/3rd-party-apis/jito-bundles</a></td></tr></tbody></table>
{% endtab %}
{% endtabs %}

### Dedicated

A private Solana endpoint with isolated capacity, custom limits, and your choice of regions, Yellowstone gRPC included. It fits a single high-throughput workload that needs full bandwidth, flat streaming costs, and the lowest latency when colocated. gRPC streaming nodes are the most common dedicated setup. See [dedicated nodes](https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/dedicated-nodes).

### Validator and staking

We operate Solana validators and offer two paths for institutional staking. Run a white-label validator under your own brand, which makes sense for treasuries and funds with large stake. Or delegate to our private trusted validator (from 2,500 SOL) and receive block rewards, MEV, and payouts weekly in USDC. For the full picture, read the [guide to white-label validators](https://blog.triton.one/comprehensive-guide-to-white-label-validators-on-solana/), or see [validator services](https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/validator-services).

## Common builds

Not sure which products you need? Pick the builder path that matches your app. Each guide walks the full stack for that build, end to end.

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-arrow-trend-up">:arrow-trend-up:</i> <strong>Trading and market making</strong></td><td>Real-time DEX data, earliest signal, priority fees, and reliable transaction landing.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/guides/solana/readme/trading-and-market-making">https://kate-6.gitbook.io/triton-one-docs-v5/guides/solana/readme/trading-and-market-making</a></td></tr><tr><td><i class="fa-wallet">:wallet:</i> <strong>Wallet or consumer app</strong></td><td>Balances, history, NFT portfolio, and live updates for wallets and consumer apps.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/guides/solana/readme/wallet-or-consumer-app">https://kate-6.gitbook.io/triton-one-docs-v5/guides/solana/readme/wallet-or-consumer-app</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
