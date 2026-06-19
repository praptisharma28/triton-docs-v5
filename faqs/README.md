---
description: >-
  What Triton is, how Project Yellowstone fits, getting an account, plans and
  payments, and the Triton validator.
---

# General

Common questions about Triton, our products, plans, payments, and how to start.

## About Triton

What Triton is, what we build, and how Project Yellowstone fits into the stack.

<details>

<summary>What is Triton One?</summary>

Triton runs RPC nodes, private validators, and data-streaming infrastructure across Solana, Sui, Monad, and other networks, for teams running real-time trading and market-making, large-scale indexing, and validator operations.

</details>

<details>

<summary>What is Project Yellowstone?</summary>

Project Yellowstone is Triton's suite of open-source Solana data tools, each named after a geyser at Yellowstone National Park. Together they cover real-time streaming, historical ledger access, indexed reads, and transaction sending.

| Tool           | What it is                                                                                                                                                                                                                  |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Dragon's Mouth | gRPC streaming from the validator's Geyser plugin: subscribe to account, transaction, slot, and block updates in real time over Protobuf. Built for backends.                                                              |
| Whirligig      | A faster, more reliable drop-in for the standard Solana node WebSocket (account, program, and signature subscriptions), for front-end and browser apps.                                                                    |
| Fumarole       | A reliable streaming layer over Dragon's Mouth with roughly 4-day retention and failover, so a dropped client can reconnect and replay missed data with no gaps.                                                           |
| Cloudbreak     | The RPC 2.0 Accounts module: a purpose-built index that serves `getProgramAccounts` and SPL token queries up to 20x faster for programs with millions of accounts, through the standard JSON-RPC methods.                  |
| Old Faithful   | An open-source, public archive of Solana's full ledger from genesis, built with the Solana Foundation and served through decentralised storage. Triton routes historical `getBlock` and `getTransaction` calls to it automatically. |
| Superbank      | The RPC 2.0 historical module: an open-source Rust workspace that ingests the full Solana ledger, stores it in ClickHouse, and serves it as spec-compliant JSON-RPC (`getTransaction`, `getSignaturesForAddress`, plus the `getTransactionsForAddress` extension). The end-to-end historical stack (ingest, store, serve), succeeding Old Faithful. |
| Jet            | Triton's direct-to-TPU transaction-sending engine: it forwards transactions straight to validator TPUs over QUIC, with stake-weighted routing (SWQoS) built in. Open source.                                               |
| Shield         | MEV-protection policies: attach an on-chain allow- or block-list of validators to a transaction so it is only forwarded to validators you trust. Works through standard `sendTransaction` or the Jet engine.               |

</details>

<details>

<summary>What other products does Triton run outside Project Yellowstone?</summary>

| Product           | What it is                                                                                              |
| ----------------- | ------------------------------------------------------------------------------------------------------- |
| Sui & Monad       | Full RPC nodes for Sui and Monad, on mainnet and testnet.                                                |
| Eclipse & PythNet | RPC nodes for Eclipse and PythNet mainnet.                                                               |
| Photon            | Indexer for ZK-compressed accounts (Light Protocol): query compressed accounts, token balances, and validity proofs. |
| Metaplex DAS API  | The Metaplex Digital Asset Standard API: read NFTs, compressed NFTs, and SPL / Token-2022 tokens through one API. |
| Pyth Hermes       | A hosted Pyth Hermes endpoint for streaming real-time Pyth price updates.                                |
| Jito              | Jito-enabled RPC: submit and simulate atomic transaction bundles.                                        |
| Titan Swap API    | A swap API serving real-time streaming quotes and routes over WebSocket, from the Argos meta-aggregator. |

</details>

## Getting started

How to set up an account, decide on a plan, and add users to your customer dashboard.

<details>

<summary>How can I get started with Triton services? / How do I get an account?</summary>

You can self-onboard at [customers.triton.one](https://customers.triton.one/users/sign-up) in about 2 minutes. For a dedicated gRPC node or other custom requirements, [contact sales](https://triton.one/contact).

</details>

<details>

<summary>How can I decide which service fits my traffic needs best?</summary>

Our docs cover comparisons for streaming, reading state, and sending transactions, so you can match the service to your traffic shape. For tailored guidance, [contact sales](https://triton.one/contact). If you already have an account, contact support by clicking the chat icon in the bottom right of your [customer dashboard](https://customers.triton.one).

</details>

<details>

<summary>How can I add a user to my Triton customer dashboard?</summary>

Open your [customer dashboard](https://customers.triton.one), scroll to the **Users** section in the main tab, click **Add users**, and invite a teammate by email.

</details>

<details>

<summary>Do you require long-term contracts?</summary>

On pay-as-you-go, there is no commitment beyond your $125 prepaid deposit, which is valid for one year of usage. For invoiced customers, standard billing is month-to-month; to cancel, give one calendar month's notice.

</details>

<details>

<summary>Can I change my plan later?</summary>

On Triton's shared infrastructure there are no plan tiers: every customer gets the same features, services, and flexible limits regardless of size, so there is nothing to upgrade between. On pay-as-you-go you can top up your balance at any time; for invoiced customers, we adjust the plan based on actual usage.

</details>

<details>

<summary>Does Triton offer a free trial to test its services?</summary>

We do not run a separate free trial. The $125 pay-as-you-go deposit serves the same purpose: a year of testing and running on production infrastructure at standard rates. Sign up at [customers.triton.one](https://customers.triton.one/users/sign-up). Trials are not available on dedicated nodes, which run on custom bare-metal servers with significant upfront cost and multi-month vendor commitments; to evaluate Triton first, start on shared infrastructure and move to dedicated later.

</details>

<details>

<summary>Can I run custom software on Triton's nodes?</summary>

No. We run managed services, so no one outside the Triton team has access to the infrastructure and we do not provide SSH access. On a dedicated node, we can set up supported add-ons on request.

</details>

## Products and services

Questions about Triton's services beyond Solana RPC.

<details>

<summary>What Pyth oracle products do you offer?</summary>

We provide PythNet RPC and Hermes for streaming price updates. We do not offer benchmarks. For publishers, we offer dedicated RPC to publish onto PythNet and validators to support the Pyth Network. We work closely with Douro Labs, the Pyth team, to ensure smooth onboarding.

</details>

<details>

<summary>How can I access Pyth Hermes?</summary>

Point your endpoint at Hermes and PythNet. The path should look like:

```
https://<endpoint>.mainnet.pythnet.rpcpool.com/<secret token>/hermes/v2/<rest of the call>
```

The most common mistake is omitting the token, `v2`, or `hermes` from the path.

</details>

<details>

<summary>Does Triton have a validator?</summary>

Yes. You can stake directly with our Private Trusted Validator and earn weekly USDC rewards; the minimum delegation is 2,500 SOL. It is built for teams with larger delegations who want steady income to cover operating expenses, with white-glove setup for custodians and stake pools.

</details>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
