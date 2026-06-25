---
description: >-
  Send Solana transactions straight to validator TPUs over QUIC with the
  standalone Yellowstone Jet TPU client in Rust.
---

# Yellowstone Jet TPU client

The Yellowstone Jet TPU client is a standalone Rust crate that sends transactions directly to validator TPUs over QUIC. It is the sending core of [Yellowstone Jet](https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/jet-sender), the open-source relay engine behind the Cascade Marketplace, pulled out into a library you can drop into your own stack.

Writing a TPU client from scratch means handling every Agave release, managing QUIC connections, tracking the leader schedule, and caching connections without lock contention. This crate does all of that, and exposes the hooks a default client leaves out.

* Crate: [`yellowstone-jet-tpu-client`](https://crates.io/crates/yellowstone-jet-tpu-client)
* API docs: [docs.rs](https://docs.rs/yellowstone-jet-tpu-client)
* Source: [github.com/rpcpool/yellowstone-jet](https://github.com/rpcpool/yellowstone-jet)

## When to use it

Anza's `tpu-client-next` shares much of its design and solves the performance problems of the legacy `ConnectionCache`. Reach for the Jet TPU client instead when you run arbitrage bots, liquidation engines, or custom routing, and need control the default client does not expose:

* **Transaction tracking.** Register a callback that fires when a transaction frame is written, fails, or is dropped by the internal event loop. Use it for custom retry logic, logging, and debugging.
* **Custom TPU contact info.** Bypass the gossip-provided contact info and supply your own TPU addresses.
* **Arbitrary remote peers.** Bypass the leader schedule entirely to implement your own offloading or routing logic.
* **Multi-step identity updates.** Rotate the sending identity through a controlled, multi-step process while keeping every sub-module in sync.
* **Allowlist and blocklist support.** Pass a custom blocklist when you call `send_txn_with_blocklist`. To gate sending on on-chain validator policies, see [Protect transactions from MEV with Shield](protect-transactions-with-shield).

## Prerequisites

* Rust and Cargo, edition 2021.
* A Triton RPC endpoint and a Yellowstone gRPC endpoint with an x-token. [Get an endpoint](https://customers.triton.one/onboarding).
* An identity keypair for the sender. The example below falls back to a random keypair if you do not supply one.

## Set up the project

Create a new binary crate and add the dependencies. The client pulls Agave's Solana crates at version 3.

```toml
[package]
name = "example-jet-tpu-client"
version = "0.1.0"
edition = "2021"

[dependencies]
bincode = "1.3.3"
clap = { version = "4.5.51", features = ["derive"] }
dotenvy = "0.15.0"
futures = "0.3.31"

# Agave crates
solana-client = "3.0.0"
solana-rpc-client-api = "3.0.0"
solana-rpc-client = "3.0.0"
solana-system-interface = { version = "3.0.0", features = ["bincode"] }
solana-bincode = "3.0.0"
solana-pubkey = "3.0.0"
solana-keypair = "3.0.0"
solana-account = "3.0.0"
solana-hash = "3.0.0"
solana-signature = "3.0.0"
solana-signer = "3.0.0"
solana-transaction = "3.0.0"
solana-commitment-config = "3.0.0"
solana-transaction-error = "3.0.0"
solana-message = "3.0.0"

thiserror = "2.0.17"
tokio = "1.48.0"
yellowstone-jet-tpu-client = { version = "0.1.0", features = ["yellowstone-grpc"] }
```

## Imports and CLI arguments

The example is a small CLI that sends lamports to any Solana address, reading its endpoints from arguments or a `.env` file.

In `src/main.rs`, start with the imports:

```rust
use {
    clap::Parser,
    solana_client::nonblocking::rpc_client::RpcClient,
    solana_commitment_config::CommitmentConfig,
    solana_hash::Hash,
    solana_keypair::{Keypair, Signature},
    solana_message::{VersionedMessage, v0},
    solana_pubkey::Pubkey,
    solana_signer::Signer,
    solana_system_interface::instruction::transfer,
    solana_transaction::versioned::VersionedTransaction,
    std::{env, path::PathBuf, sync::Arc, vec},
    yellowstone_jet_tpu_client::{
        core::TpuSenderResponse,
        yellowstone_grpc::sender::{
            Endpoints, NewYellowstoneTpuSender, YellowstoneTpuSender,
            create_yellowstone_tpu_sender_with_callback,
        },
    },
};
```

Define the CLI arguments:

```rust
#[derive(clap::Parser, Debug)]
struct Args {
    /// Path to .env file to load
    #[clap(long, short)]
    dotenv: Option<PathBuf>,

    /// Endpoint to the RPC service
    #[clap(long, short)]
    rpc: Option<String>,

    /// Endpoint to the Yellowstone gRPC service
    #[clap(long, short)]
    grpc: Option<String>,

    /// X-Token for the Yellowstone gRPC service
    x_token: Option<String>,

    /// Path to the identity keypair file
    identity: Option<PathBuf>,

    /// Recipient pubkey
    #[clap(long)]
    recipient: Option<String>,
}
```

## Write the send function

Build the transfer, sign it, bincode-serialize it, and hand it to the sender. The signature you pass to `send_txn` is what the callback uses to report the outcome:

```rust
async fn send_lamports(
    mut tpu_sender: YellowstoneTpuSender,
    identity: &Keypair,
    recipient: &Pubkey,
    lamports: u64,
    latest_blockhash: Hash,
) -> Signature {
    let instructions = vec![transfer(&identity.pubkey(), &recipient, lamports)];

    let transaction = VersionedTransaction::try_new(
        VersionedMessage::V0(
            v0::Message::try_compile(
                &identity.pubkey(),
                &instructions,
                &[],
                latest_blockhash,
            )
            .expect("try_compile"),
        ),
        &[&identity],
    )
    .expect("try_new");

    let signature = transaction.signatures[0];

    // The TPU sender expects bincoded transaction data.
    // Other byte containers are supported too: Bytes, Arc<Vec<u8>>, Arc<[u8]>.
    let bincoded_txn: Vec<u8> =
        bincode::serialize(&transaction).expect("bincode::serialize");

    // Send to the current leader. The signature tracks the transaction's lifecycle.
    tpu_sender
        .send_txn(signature, bincoded_txn)
        .await
        .expect("send_transaction");

    signature
}
```

## Wire up main

Parse the arguments, load the optional `.env` file, resolve the endpoints and identity, then build the sender. Passing a callback channel to `create_yellowstone_tpu_sender_with_callback` lets you track each transaction's outcome:

```rust
#[tokio::main]
async fn main() {
    use std::io::Write;

    let mut out = std::io::stdout();
    let args = Args::parse();

    // Supports a .env file with RPC_ENDPOINT, GRPC_ENDPOINT, and GRPC_X_TOKEN.
    if let Ok(env_path) = args.dotenv.unwrap_or("./.env".into()).canonicalize() {
        if dotenvy::from_path(env_path).is_err() {
            eprintln!("Warning: failed to load .env file");
        }
    } else {
        eprintln!("Warning: failed to canonicalize .env file path");
    }

    let recipient_pubkey: Pubkey = match args.recipient {
        Some(recipient) => recipient.parse().expect("Failed to parse recipient pubkey"),
        None => Pubkey::new_unique(),
    };

    let rpc_endpoint = match args.rpc {
        Some(endpoint) => endpoint,
        None => env::var("RPC_ENDPOINT")
            .expect("RPC_ENDPOINT must be set in the .env file or environment"),
    };

    let grpc_endpoint = match args.grpc {
        Some(endpoint) => endpoint,
        None => env::var("GRPC_ENDPOINT")
            .expect("GRPC_ENDPOINT must be set in the .env file or environment"),
    };

    let grpc_x_token = match args.x_token {
        Some(x_token) => Some(x_token),
        None => match env::var("GRPC_X_TOKEN") {
            Ok(token) => Some(token),
            Err(_) => {
                eprintln!("Warning: GRPC_X_TOKEN not set in the .env file or environment");
                None
            }
        },
    };

    let identity = match args.identity {
        Some(path) => solana_keypair::read_keypair_file(path)
            .expect("Failed to read identity keypair file"),
        None => {
            if let Ok(identity_path) = env::var("IDENTITY") {
                solana_keypair::read_keypair_file(identity_path)
                    .expect("Failed to read identity keypair file from env")
            } else {
                eprintln!("IDENTITY not set, using a new random identity");
                Keypair::new()
            }
        }
    };

    let rpc_client = Arc::new(RpcClient::new_with_commitment(
        rpc_endpoint.clone(),
        CommitmentConfig::confirmed(),
    ));

    writeln!(out, "Using identity: {}", identity.pubkey()).expect("writeln");

    let endpoints = Endpoints {
        rpc: rpc_endpoint,
        grpc: grpc_endpoint,
        grpc_x_token,
    };

    // Pass a callback so you can track the success or failure of the transaction.
    let (callback_tx, mut callback_rx) = tokio::sync::mpsc::unbounded_channel();

    let NewYellowstoneTpuSender {
        sender,
        related_objects_jh: _,
    } = create_yellowstone_tpu_sender_with_callback(
        Default::default(),
        identity.insecure_clone(),
        endpoints,
        callback_tx,
    )
    .await
    .expect("tpu-sender");

    const LAMPORTS: u64 = 1000;

    let latest_blockhash = rpc_client
        .get_latest_blockhash()
        .await
        .expect("get_latest_blockhash");

    let signature = send_lamports(
        sender,
        &identity,
        &recipient_pubkey,
        LAMPORTS,
        latest_blockhash,
    )
    .await;

    let TpuSenderResponse::TxSent(resp) = callback_rx
        .recv()
        .await
        .expect("receive tpu sender response")
    else {
        panic!("unexpected tpu sender response");
    };

    assert!(resp.tx_sig == signature, "unexpected tx signature in response");

    writeln!(
        &mut out,
        "sent transaction with signature `{}` to validator `{}`",
        resp.tx_sig, resp.remote_peer_identity
    )
    .expect("writeln");
}
```

## Run it

Pass the endpoints as arguments, or put them in a `.env` file with `RPC_ENDPOINT`, `GRPC_ENDPOINT`, and `GRPC_X_TOKEN`:

```bash
cargo run -- --recipient <recipient-pubkey>
```

The callback returns a `TxSent` response carrying the signature and the identity of the validator the transaction reached.

## What's next

This client gives you the primitives to build your own routing logic without reinventing QUIC handling or leader tracking. To send only to validators you trust and skip known bad actors, combine it with on-chain Shield policies: [Protect transactions from MEV with Shield](protect-transactions-with-shield).

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
