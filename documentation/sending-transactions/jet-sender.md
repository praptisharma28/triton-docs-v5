---
description: A Rust library that sends Solana transactions straight to validator TPUs over QUIC.
---

# Jet TPU client

`yellowstone-jet-tpu-client` is a standalone Rust library that sends Solana transactions straight to validator TPUs over QUIC. It carries the SWQoS handling, QUIC connection caching, and leader tracking from Jet sender, Triton's production transaction engine, extracted so you can use the sending logic without running the whole engine.

## Use the Jet TPU client when

You need control the standard senders do not give you: per-transaction lifecycle callbacks, custom TPU targeting, routing around the leader schedule, or enforcing [Yellowstone Shield](shield-mev-protection.md) policies in your own code. It is built for arbitrage bots, liquidation engines, and custom routers.

Use Triton RPC `sendTransaction` instead when you only want reliable delivery: SWQoS is applied and there is no client to run. See Best practices.

## What it adds over `tpu-client-next`

Anza's `tpu-client-next` is a solid client, but it omits features that high-frequency and custom-routing workloads need. The Jet client adds:

* **Transaction tracking.** Register a callback that fires when a transaction is written to the wire, fails, or is dropped by the internal event loop. Use it for custom retry logic, logging, and debugging.
* **Override TPU contact info.** Bypass the gossip-provided addresses and supply your own.
* **Send to arbitrary peers.** Bypass the leader schedule entirely for custom offloading or routing.
* **Multi-step identity updates.** Rotate the sending identity across Jet's sub-modules through a controlled sequence.
* **Allowlist and blocklist routing.** Pass a blocklist to `send_txn_with_blocklist`, or enforce on-chain Shield policies with `send_txn_with_shield_policies`.

SWQoS, QUIC quirks, and Agave release changes are handled for you.

## Before you start

The client depends on Triton's patched Solana crates. Pin them with a `[patch.crates-io]` block pointing at `rpcpool/solana-public` at tag `v3.0.6-triton-public`, or the build fails to resolve a consistent Solana version. The [Shield MEV protection](shield-mev-protection.md) page has the full patch block.

You need an RPC endpoint and a Yellowstone gRPC endpoint with an `x-token`, both from your [dashboard](https://customers.triton.one).

## Quickstart: send a transfer

`Cargo.toml`:

```toml
[dependencies]
yellowstone-jet-tpu-client = { version = "0.1.0", features = ["yellowstone-grpc"] }
solana-client = "3.0"
solana-keypair = "3.0"
solana-pubkey = "3.0"
solana-signer = "3.0"
solana-message = "3.0"
solana-system-interface = { version = "3.0", features = ["bincode"] }
solana-commitment-config = "3.0"
solana-transaction = "3.0"
bincode = "1.3"
tokio = { version = "1", features = ["full"] }
```

Create the sender with a tracking callback, then send a bincoded transaction. The callback reports the lifecycle of every transaction you send.

```rust
use {
    solana_client::nonblocking::rpc_client::RpcClient,
    solana_commitment_config::CommitmentConfig,
    solana_keypair::Keypair,
    solana_message::{v0, VersionedMessage},
    solana_pubkey::Pubkey,
    solana_signer::Signer,
    solana_system_interface::instruction::transfer,
    solana_transaction::versioned::VersionedTransaction,
    std::sync::Arc,
    yellowstone_jet_tpu_client::{
        core::TpuSenderResponse,
        yellowstone_grpc::sender::{
            create_yellowstone_tpu_sender_with_callback, Endpoints, NewYellowstoneTpuSender,
        },
    },
};

#[tokio::main]
async fn main() {
    let identity = Keypair::new(); // in production, read from a keypair file
    let recipient = Pubkey::new_unique();

    let rpc = Arc::new(RpcClient::new_with_commitment(
        "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>".into(),
        CommitmentConfig::confirmed(),
    ));

    let endpoints = Endpoints {
        rpc: "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>".into(),
        grpc: "https://<your-endpoint>.mainnet.rpcpool.com".into(),
        grpc_x_token: Some("<your-token>".into()),
    };

    let (callback_tx, mut callback_rx) = tokio::sync::mpsc::unbounded_channel();
    let NewYellowstoneTpuSender { mut sender, .. } =
        create_yellowstone_tpu_sender_with_callback(
            Default::default(),
            identity.insecure_clone(),
            endpoints,
            callback_tx,
        )
        .await
        .expect("tpu-sender");

    let blockhash = rpc.get_latest_blockhash().await.expect("blockhash");
    let instructions = vec![transfer(&identity.pubkey(), &recipient, 1000)];
    let transaction = VersionedTransaction::try_new(
        VersionedMessage::V0(
            v0::Message::try_compile(&identity.pubkey(), &instructions, &[], blockhash)
                .expect("compile"),
        ),
        &[&identity],
    )
    .expect("sign");

    let signature = transaction.signatures[0];
    let wire = bincode::serialize(&transaction).expect("serialize");

    // Send to the current leader. The signature tracks the transaction's lifecycle.
    sender.send_txn(signature, wire).await.expect("send");

    if let TpuSenderResponse::TxSent(resp) = callback_rx.recv().await.expect("callback") {
        println!("sent {} to validator {}", resp.tx_sig, resp.remote_peer_identity);
    }
}
```

`send_txn` also accepts `Bytes`, `Arc<Vec<u8>>`, and `Arc<[u8]>` for the wire payload. For the batteries-included CLI version and every constructor option, see the [crate docs](https://docs.rs/yellowstone-jet-tpu-client).

## Add MEV protection

To route around untrusted validators, enable the `shield` feature and call `send_txn_with_shield_policies` with an on-chain policy address. The client checks the current leader against the policy and only sends to allowed validators, skipping the rest. See [Shield MEV protection](shield-mev-protection.md) for the workspace setup and a complete example.

## Resources

* Crate: [yellowstone-jet-tpu-client](https://crates.io/crates/yellowstone-jet-tpu-client)
* API docs: [docs.rs/yellowstone-jet-tpu-client](https://docs.rs/yellowstone-jet-tpu-client)
* Engine repository: [yellowstone-jet](https://github.com/rpcpool/yellowstone-jet)

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
