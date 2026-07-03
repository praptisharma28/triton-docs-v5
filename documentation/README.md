---
description: Triton's Solana infrastructure stack and product overview.
---

# Overview

Triton has built on Solana since the testnet, as one of the network's original validators. Today we run premium, globally distributed bare-metal infrastructure for many of the ecosystem's most demanding teams across DeFi, trading, and institutions.

Alongside running infrastructure, we author core public tooling the ecosystem depends on. Our best-known work:

* **RPC 2.0:** a ground-up rebuild of Solana's read layer for faster, cheaper, more expressive account and ledger queries
* **Yellowstone gRPC (Dragon's Mouth):** Geyser-based gRPC streaming for account, transaction, slot, and block updates, now the de facto Solana streaming standard
* **Old Faithful:** the only complete, verified, public Solana archive, stored as content-addressable files (CARs), queryable and streamable back to genesis
* **Metaplex DAS API:** two years of open-source enhancements to the ecosystem's read API for NFTs, cNFTs, and tokens: a new gRPC-based ingestion pipeline, a reworked query layer, and token parsers, with `getAsset` now returning in ~50 ms at P90

## Triton stack

We offer a complete Solana stack for any use case: shared infrastructure, dedicated nodes, and validator and staking services.

### Shared infrastructure

Every Triton subscription gives you the complete feature set, free devnet, and flexible connection and rate limits. We have split Solana's read layer into purpose-built, independent components that scale automatically and absorb spikes seamlessly. Combined with global distribution and instant auto-failover, the shared cluster is the right answer for most workloads.

{% tabs %}
{% tab title="Reading state" %}
<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-server">:server:</i> <strong>Standard RPC</strong></td><td>Every standard Solana JSON-RPC method, served from premium bare-metal nodes.</td><td><a href="https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana">https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana</a></td></tr><tr><td><i class="fa-database">:database:</i> <strong>Cloudbreak</strong></td><td>Indexed account reads: account and token queries up to 99% faster with tailored dynamic indexes.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/cloudbreak-indexed-accounts">https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/cloudbreak-indexed-accounts</a></td></tr><tr><td><i class="fa-image">:image:</i> <strong>DAS API</strong></td><td>One API for NFTs, cNFTs, and tokens via the Metaplex Digital Asset Standard.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/metaplex-das-api">https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/metaplex-das-api</a></td></tr><tr><td><i class="fa-arrows-rotate">:arrows-rotate:</i> <strong>Account Sync</strong></td><td>One-line SDK swap that serves account reads from a local cache kept fresh by a live stream, faster and cheaper.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/account-sync">https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/account-sync</a></td></tr></tbody></table>
{% endtab %}

{% tab title="Streaming" %}
<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-radio">:radio:</i> <strong>Dragon's Mouth gRPC</strong></td><td>Geyser-fed gRPC streaming: sub-slot account, transaction, and block subscriptions, up to 400 ms faster than RPC polling.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/dragon-s-mouth-grpc">https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/dragon-s-mouth-grpc</a></td></tr><tr><td><i class="fa-fire">:fire:</i> <strong>Deshred Transactions gRPC</strong></td><td>Pre-execution tx feed from raw shreds, skipping the node's replay stage: ~20 ms ahead of regular gRPC at p75.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/deshred-transactions">https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/deshred-transactions</a></td></tr><tr><td><i class="fa-rotate-right">:rotate-right:</i> <strong>Whirligig</strong></td><td>A gRPC-backed Solana WebSocket layer for frontends, with transactionSubscribe support and high connection limits.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/whirligig-websockets">https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/whirligig-websockets</a></td></tr><tr><td><i class="fa-layer-group">:layer-group:</i> <strong>Fumarole</strong></td><td>Persistent gRPC streams: a 4-day buffer, auto-resume on disconnect, and an at-least-once delivery guarantee.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/fumarole-persistent-streams">https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/fumarole-persistent-streams</a></td></tr></tbody></table>
{% endtab %}

{% tab title="Historical" %}
<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-clock-rotate-left">:clock-rotate-left:</i> <strong>Superbank</strong></td><td>Purpose-built historical backend for easy millisecond reads across the entire ledger.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/historical-data">Superbank</a></td></tr><tr><td><i class="fa-box-archive">:box-archive:</i> <strong>Superbank streams</strong></td><td>Replay bounded slot ranges of history over gRPC with StreamBlocks and StreamTransactions.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/historical-data#how-it-works">Superbank streams</a></td></tr></tbody></table>
{% endtab %}

{% tab title="Sending transactions" %}
<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-bolt">:bolt:</i> <strong>sendTx</strong></td><td>Triton's direct HTTP send endpoint without the JSON-RPC envelope, for lower latency and no RPC client library. SWQoS included by default.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/quickstart">https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/quickstart</a></td></tr><tr><td><i class="fa-paper-plane">:paper-plane:</i> <strong>Jet sender</strong></td><td>Direct-to-leader TPU transaction-sending engine over QUIC, integrating Shield and the priority-fee API, with SWQoS out of the box.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions">https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions</a></td></tr><tr><td><i class="fa-arrow-trend-up">:arrow-trend-up:</i> <strong>Priority Fees API</strong></td><td>Percentile-based priority-fee estimates for reliable landing without overpaying.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/priority-fees-api">https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/priority-fees-api</a></td></tr><tr><td><i class="fa-code-branch">:code-branch:</i> <strong>Metis</strong></td><td>Triton-hosted Metis API for quoting, routing and swapping across 20+ DEXes, no staked JUP required.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/3rd-party-apis/metis-swap-api">https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/3rd-party-apis/metis-swap-api</a></td></tr><tr><td><i class="fa-route">:route:</i> <strong>Titan Prime</strong></td><td>Meta-aggregation of Solana liquidity, real-time quote streaming and DART routing that re-optimises the route at execution.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/3rd-party-apis/titan-swap-api">Titan Prime</a></td></tr><tr><td><i class="fa-box">:box:</i> <strong>Jito Bundle Simulation</strong></td><td>Simulate atomic transaction bundles on Triton's Jito-enabled RPC.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/3rd-party-apis/jito-bundles">https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/3rd-party-apis/jito-bundles</a></td></tr></tbody></table>
{% endtab %}
{% endtabs %}

### Dedicated nodes

A single-purpose Solana node with isolated capacity, no noisy neighbours, no rate limits, and your choice of regions for easy colocation. It fits high-throughput workloads that need full bandwidth, dedicated compute, flat streaming costs, and the lowest, steadiest backend latency.

gRPC streaming nodes are the most common dedicated setup. For other custom setups like indexing or history, we also offer managed solutions: [contact us](https://triton.one/contact). See [dedicated nodes](https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/dedicated-nodes).

### Validator and staking

For treasuries, institutions, and custodians, we run white-label validators under your brand, with full key separation and no operational overhead. For smaller stake, you can delegate to our private trusted validator (from 2,500 SOL) and receive your staking rewards, block rewards, and Jito MEV paid weekly in USDC. See [validator services](https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/validator-services).

## Common builds

Not sure which products you need? Pick the full-setup quickstart that matches what you are building.

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-arrow-trend-up">:arrow-trend-up:</i> <strong>Trading and market making</strong></td><td>Real-time DEX data, earliest signal, priority fees, and reliable transaction landing.</td><td><a href="https://app.gitbook.com/s/TpqU5Dqc6tdzY8J23dd7/solana/use-cases/trading-and-market-making">https://app.gitbook.com/s/TpqU5Dqc6tdzY8J23dd7/solana/use-cases/trading-and-market-making</a></td></tr><tr><td><i class="fa-wallet">:wallet:</i> <strong>Wallet or consumer app</strong></td><td>Balances, history, NFT portfolio, and live updates for wallets and consumer apps.</td><td><a href="https://app.gitbook.com/s/TpqU5Dqc6tdzY8J23dd7/solana/use-cases/building-a-wallet">https://app.gitbook.com/s/TpqU5Dqc6tdzY8J23dd7/solana/use-cases/building-a-wallet</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
