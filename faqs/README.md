---
description: Common questions about Triton's Solana products, and how to get started.
---

# General

## About Triton

What Triton is, what we build, and how Project Yellowstone fits into the stack.

<details>

<summary>What is Triton One?</summary>

Triton is a high-performance infrastructure provider running RPC nodes, private validators, and data streaming systems across Solana and other networks.

It delivers reliable, fast, and feature-rich blockchain infrastructure trusted by developers, validators, and traders that can't afford downtime.

We power everything from real-time trading and market-making to large-scale data indexing and validator operations.

</details>

<details>

<summary>What is Project Yellowstone?</summary>

Project Yellowstone is Triton's suite of open-source Solana data tools, each named after a geyser at Yellowstone National Park. Together, they cover real-time streaming, historical ledger access, indexed reads, and transaction sending.

| Tool                       | What it is                                                                                                                                                                                       |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Dragon's Mouth gRPC        | gRPC streaming from the validator's Geyser plugin: subscribe to account, transaction, slot, and block updates in real time over Protobuf.                                                        |
| Whirligig WebSockets       | A faster, more reliable drop-in for the standard Solana WebSocket (plus the `transactionSubscribe` extension), for frontends.                                                                         |
| Fumarole gRPC              | Persistent multiplex streams that stay complete through any disconnect, with a server-side cursor and \~4-day auto-backfill.                                                                     |
| Cloudbreak indexes         | An accounts indexing module that serves program and token account queries 99%+ faster through the standard JSON-RPC methods.                                                                     |
| Old Faithful archive       | The only public archive of Solana's complete ledger from genesis, stored as verifiable CAR files.                                                                                                |
| Superbank ledger pipeline  | An end-to-end historical stack in Rust that ingests the full Solana ledger, stores it in ClickHouse, and serves it as spec-compliant JSON-RPC (plus the `getTransactionsForAddress` extension).  |
| Jet sender                 | Direct-to-TPU transaction-sending engine, with stake-weighted QoS, Shield MEV protection, leader-aware QUIC routing, and per-request retries built in.                                           |
| Shield protection policies | On-chain allow- and block-lists of validators for safe and compliant transaction routing. Works through standard `sendTransaction` and is natively integrated with the Jet engine.                   |

</details>

<details>

<summary>What other products does Triton run outside Project Yellowstone?</summary>

| Product          | What it is                                                                                                     |
| ---------------- | -------------------------------------------------------------------------------------------------------------- |
| Sui & Monad      | Full RPC nodes and validators for Sui and Monad, across mainnet and testnet.                                   |
| Metaplex DAS API | Digital Asset Standard (DAS) API: query NFTs, compressed NFTs, and fungible tokens through one indexed request. |
| Jito             | Jito-enabled RPC: simulate atomic transaction bundles.                                                         |
| Titan Swap API   | A swap API serving real-time streaming quotes and routes over WebSocket, from the Argos meta-aggregator.       |

</details>

<details>

<summary>Does Triton have a validator?</summary>

Yes. You can stake directly with our Private Trusted Validator and earn weekly USDC rewards. It's built for teams with larger delegations (2,500 SOL+) who want a steady weekly income in stablecoins, which keeps compliance and accounting simple.

We also offer white-label setups for DAOs, custodians, and stake pools that want their own validator without the operational overhead.

</details>

## Getting started

How to set up an account, decide on a plan, and add users to your customer dashboard.

<details>

<summary>How do I get a Triton account?</summary>

You can self-onboard at [customers.triton.one](https://customers.triton.one/users/sign-up) in about 2 minutes. For a dedicated gRPC node or other custom requirements, [contact sales](https://triton.one/contact).

</details>

<details>

<summary>How can I decide which service fits my traffic needs best?</summary>

Our docs cover comparisons for streaming, reading state, and sending transactions, so you can match the service to your traffic shape. For tailored guidance, [contact sales](https://triton.one/contact).

If you already have an account, contact support by clicking the chat icon in the bottom right of your [customer dashboard](https://customers.triton.one).

</details>

<details>

<summary>Do you require long-term contracts?</summary>

On pay-as-you-go, there is no commitment beyond your $125 prepaid deposit, which is valid for one year of usage.

For invoiced customers, standard billing is month-to-month; to cancel, give one calendar month's notice.

</details>

<details>

<summary>Can I change my plan later?</summary>

On Triton's shared infrastructure there are no plan tiers: every customer gets the same features, services, and flexible limits regardless of deposit amount, so there is nothing to upgrade between.

On pay-as-you-go you can top up your balance at any time; for invoiced customers, we adjust the plan based on actual usage.

</details>

<details>

<summary>Does Triton offer a free trial to test its services?</summary>

We do not run a separate free trial. The $125 pay-as-you-go deposit (self-onboard at [customers.triton.one](https://customers.triton.one/users/sign-up)) gives you a year of testing and running on production infrastructure with access to all the features.

Trials are not available on dedicated nodes, which run on custom bare-metal servers with significant upfront cost and multi-month vendor commitments; to evaluate Triton first, start on shared infrastructure and move to dedicated later.

</details>

<details>

<summary>Can I run custom software on Triton's nodes?</summary>

No. We run managed services, so no one outside the Triton team has access to the infrastructure, and we do not provide SSH access. On a dedicated node, we can set up supported add-ons on request.

</details>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
