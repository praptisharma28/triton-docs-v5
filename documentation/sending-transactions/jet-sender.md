---
description: "Triton's managed transaction-sending engine: SWQoS, leader-aware QUIC delivery, Shield, and priority fees on every send."
---

# Jet sender

Jet sender is Triton's production transaction-sending engine, and every transaction submitted through a Triton endpoint routes through it. It applies stake-weighted quality of service (SWQoS), tracks the leader schedule, caches QUIC connections to validators, and forwards your transaction straight to the current leader's TPU. It runs on isolated infrastructure that does not compete with your RPC reads, and it integrates [Yellowstone Shield](shield-mev-protection.md) and the [Priority fees API](priority-fees-api.md) out of the box.

There is nothing to install or run: send the way you already do, with `sendTransaction` or [`/sendtx`](sending-transactions/quickstart.md), and Jet sender handles delivery. For the full comparison of submission paths, see [Sending transactions](../sending-transactions.md).

## Pricing

Jet sender is billed as standard RPC: each send or simulate call (`sendTransaction`, `/sendtx`, `simulateTransaction`) counts as one RPC request, at `$0.08 / GB` of bandwidth plus `$10 / million` calls.

## Self-hosting: the Jet TPU client

The logic behind Jet sender is open source as the standalone `yellowstone-jet-tpu-client` Rust library, for teams that want to run it themselves for client-side control: per-transaction lifecycle callbacks, custom TPU targeting, routing around the leader schedule, sending to arbitrary peers, or enforcing [Shield](shield-mev-protection.md) policies in your own code. It is built for arbitrage bots, liquidation engines, and custom routers. It depends on Triton's patched Solana crates, pinned with a `[patch.crates-io]` block pointing at `rpcpool/solana-public` at tag `v3.0.6-triton-public`. See the [crate docs](https://docs.rs/yellowstone-jet-tpu-client) for the full quickstart and every constructor option.

## Resources

* Crate: [yellowstone-jet-tpu-client](https://crates.io/crates/yellowstone-jet-tpu-client)
* API docs: [docs.rs/yellowstone-jet-tpu-client](https://docs.rs/yellowstone-jet-tpu-client)
* Engine repository: [yellowstone-jet](https://github.com/rpcpool/yellowstone-jet)

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
