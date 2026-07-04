---
description: A Solana validator run on Triton infrastructure under your own brand and keys.
---

# White-label validators

Running a validator helps protect and decentralize the Solana network and earns continued rewards through the stake you can attract to your validator.

Our hosted validators run on the infrastructure we have built as a top RPC provider and long-time validator operator on Solana.

On the validators we run, we ensure:

* Access by a limited team.
* Node identity keys in-memory only.
* 24/7 monitoring to ensure consistent voting.

Triton One owns the validator identity key. You or your team control the vote account withdrawal authority. Control over the withdrawal authority means you have complete control over the vote account.

Before you commit to running a validator, you should make sure that:

1. You are willing to fund vote fees until you have attracted enough stake to break even; this is ~1 SOL/day.
2. You can attract enough delegations to break even (TX fees - voting expenses) and make a profit eventually. Currently, this is in the range of 375,000 delegated SOL.

If you merely wish to access staking rewards, you can also stake with existing validators without running your own; see [validators.app](https://validators.app/) for details about existing validators and stake pools.

## Supported validator clients

Validators deployed through Triton are compatible with all current Solana validator clients that adhere to protocol specifications. The choice of client is left entirely to the validator owner. Triton monitors compatibility and vote performance across supported clients, which include:

* **Agave**: the official reference implementation of the Solana validator, maintained by [Anza](https://anza.xyz/). Agave is feature-complete and receives all protocol updates first, serving as the baseline for all other clients.
* **Jito-Solana**: a fork of Agave maintained by [Jito Labs](https://jito.network/), integrating an MEV auction system. This client allows validators to receive transaction bundles from a private relay and earn additional rewards through MEV tips, with minimal configuration overhead.
* **Paladin**: a fork of Jito-Solana maintained by the [Paladin](https://github.com/paladin-bladesmith) team. It offers stricter bundle filtering and supports a protected transaction pathway that mitigates frontrunning and sandwich attacks. It is designed to improve fairness in MEV distribution while remaining fully compatible with Solana's consensus.
* **Frankendancer**: a hybrid validator client maintained by [Jump Crypto](https://jumpcrypto.com/firedancer/), combining a Firedancer networking and block ingestion stack with consensus execution provided by Agave. It is engineered for high performance and low-latency block processing and is already in production on mainnet with active participation.

Each of these clients is capable of full validator participation. Triton validates client behavior continuously to ensure correct protocol operation.

To participate in the Triton SWQoS (Cascade Routing Layer), which provides a prioritized stream of transactions and the opportunity to generate additional revenue, non-Triton validators must configure their infrastructure to accept authenticated traffic from Triton's transaction distribution layer. This system is opt-in and compatible with supported clients. For configuration and operational details, see [Providing Transaction Bandwidth](https://docs.triton.one/chains/solana/cascade/providing-transaction-bandwidth).

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-key">:key:</i> <strong>Vote account setup</strong></td><td>The three keys behind a validator, and how the vote account is created and controlled.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/validator-services/white-label-validators/vote-account-setup">Vote account setup</a></td></tr><tr><td><i class="fa-shield">:shield:</i> <strong>Node identity protection</strong></td><td>How Triton keeps the node identity key in-memory only, never stored unencrypted.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/validator-services/white-label-validators/node-identity-protection">Node identity protection</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
