---
description: >-
  Combine on-chain Yellowstone Shield policies with the Jet TPU client so your
  transactions only reach validators you trust.
---

# Protect transactions from MEV with Shield

Solana's leader schedule is deterministic, so you often know exactly which validator will process your transaction. If that leader is a bad actor, your transaction still routes to it by default, which leaves you exposed to sandwiching, frontrunning, censorship, and slow block production.

[Yellowstone Shield](https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/shield-mev-protection) addresses this with on-chain allowlists and blocklists for validators. Combined with the [Yellowstone Jet TPU client](yellowstone-jet-tpu-client), your application connects directly to validator TPUs, checks the current leader against your Shield policies, and only sends to validators you allow. The result is RPC-independent transaction routing where you decide which validators are allowed to see your transactions.

* Shield program: [github.com/rpcpool/yellowstone-shield](https://github.com/rpcpool/yellowstone-shield)
* Policy store crate: [`yellowstone-shield-store`](https://crates.io/crates/yellowstone-shield-store)
* Policy explorer: [validators.app/yellowstone-shield](https://www.validators.app/yellowstone-shield)

## How validator-level MEV happens

A transaction has to reach the current block leader to be processed. When that leader misbehaves, default transaction sending gives you no say in the matter. Common behaviours worth routing around:

* **Sandwich attacks:** inserting transactions before and after yours to extract value.
* **Frontrunning:** copying your transaction and executing it first.
* **Censorship:** selectively dropping transactions.
* **Slow block production:** delaying your transactions.

Shield lets you decide, ahead of time, which validators are allowed to handle your transactions.

## How Shield works

Yellowstone Shield is an on-chain access-control program. You define policies, and each policy lives on-chain as a Program Derived Address (PDA) that anyone with the address can use:

* **Allowlists** send only to trusted validators.
* **Blocklists** never send to known bad actors.

The TPU client reads these policies through a `PolicyStore` that caches them from chain and keeps them current over gRPC.

## Community policies

The community already maintains several Shield policies. Browse them at [validators.app/yellowstone-shield](https://www.validators.app/yellowstone-shield):

| Policy                                | Type  | Validators | Purpose                       |
| ------------------------------------- | ----- | ---------- | ----------------------------- |
| Top 25 Validators by Stake (TV25)     | Allow | 25         | Only send to top validators   |
| Sandwiched.me Sandwicher List (DROPS) | Deny  | 3          | Block known MEV sandwichers   |
| Jito Drop List (DROPJ)                | Deny  | 17         | Block Jito blocklist validators |
| Marinade Drop List (DROPM)            | Deny  | 73         | Block Marinade's blocklist    |
| Slow Block Producers (SLOWAF)         | Deny  | 22         | Avoid slow validators         |

You can use these as they are, or create your own.

## RPC Shield or the TPU client?

If you use Triton RPC with Cascade, you can apply Shield without writing any Rust by adding a policy to your `sendTransaction` call:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sendTransaction",
  "params": [
    "<base64_transaction>",
    {
      "encoding": "base64",
      "forwardingPolicies": ["<your_policy_pda>"]
    }
  ]
}
```

That route is the quickest to integrate, but it depends on the RPC's forwarding logic and gives you no direct control over routing. The TPU client connects to validators yourself, so you decide everything. Pick based on your workload:

| Feature                      | RPC Shield (Cascade)         | TPU client Shield            |
| ---------------------------- | ---------------------------- | ---------------------------- |
| Setup                        | Easy: add one parameter      | Moderate: requires Rust      |
| RPC dependency               | Needs a Shield-enabled RPC   | None                         |
| Validator connection         | Through the RPC              | Direct QUIC connections      |
| Control over routing         | Limited to the RPC's logic   | Full                         |
| Transaction prioritisation   | RPC-controlled               | Yours                        |
| Best for                     | Quick integration, web apps  | High-throughput apps, MEV-sensitive routing |

The rest of this guide covers the TPU client route.

## Prerequisites

* Rust and Cargo, edition 2021.
* A Triton RPC endpoint and a Yellowstone gRPC endpoint with an x-token. [Get an endpoint](https://customers.triton.one/onboarding).
* The [Yellowstone Jet TPU client](yellowstone-jet-tpu-client) set up, or follow the project setup below.

## Choose or create a policy

### Option A: use an existing policy

The quickest start is a community policy. For example, to block known MEV sandwichers with the Sandwiched.me list (DROPS):

{% tabs %}
{% tab title="Rust" %}
```rust
use solana_pubkey::Pubkey;
use std::str::FromStr;

// Sandwiched.me Sandwicher List (DROPS), blocks 3 known MEV attackers.
let shield_policy = Pubkey::from_str("xMTozeQTEX2MR9KUon8vjg17U5Q9459RSASE3wy5eNB")
    .expect("valid pubkey");
```
{% endtab %}
{% endtabs %}

Browse more at [validators.app/yellowstone-shield](https://www.validators.app/yellowstone-shield).

### Option B: create your own policy

For custom control, create a policy with the Yellowstone Shield CLI.

**1. Build the CLI:**

```bash
git clone https://github.com/rpcpool/yellowstone-shield
cd yellowstone-shield
cargo build --release --bin yellowstone-shield-cli
```

**2. Prepare policy metadata.** Create a JSON file describing the policy, upload it to IPFS or Arweave, and keep the URL:

```json
{
  "name": "My MEV blocklist",
  "symbol": "MEVBLOCK",
  "description": "Blocks validators known for sandwich attacks",
  "image": "https://the-image-url.com/shield.png",
  "external_url": "https://your-website.com"
}
```

**3. Create the policy** and save the returned PDA:

```bash
# A blocklist denies the listed validators.
./target/release/yellowstone-shield-cli policy create \
  --strategy Deny \
  --name "My MEV blocklist" \
  --symbol "MEVBLOCK" \
  --uri "https://the-metadata-url.json"

# Output: Policy Mint Address: <YOUR_POLICY_PDA>
```

**4. Add validators.** Put one validator pubkey per line in `validators-to-block.txt`, then add them:

```bash
./target/release/yellowstone-shield-cli identities add \
  --mint <YOUR_POLICY_PDA> \
  --identities-path validators-to-block.txt
```

The policy is now live on-chain and ready to use.

## Set up the project

The `yellowstone-jet-tpu-client` requires Triton's patched Solana crates. Use a Cargo workspace with `[patch.crates-io]` so every dependency, including transitive ones from `yellowstone-shield-store`, resolves to the same patched versions.

Workspace root `Cargo.toml`:

```toml
[workspace]
members = ["example"]
resolver = "2"

[workspace.dependencies]
yellowstone-jet-tpu-client = { version = "0.1.0-rc4", features = ["yellowstone-grpc", "shield"] }
yellowstone-shield-store = "0.9.1"
yellowstone-grpc-client = "10.2.0"
solana-client = "3.0"
solana-keypair = "3.0"
solana-pubkey = "3.0"
solana-signature = "3.0"
solana-transaction = "3.0"
solana-message = "3.0"
solana-system-interface = { version = "3.0", features = ["bincode"] }
solana-commitment-config = "3.0"
solana-signer = "3.0"
tokio = { version = "1", features = ["full"] }
bincode = "1"
anyhow = "1"

[patch.crates-io]
# Required patches from Triton's Solana fork.
solana-rpc-client = { git = "https://github.com/rpcpool/solana-public.git", tag = "v3.0.6-triton-public" }
solana-rpc-client-api = { git = "https://github.com/rpcpool/solana-public.git", tag = "v3.0.6-triton-public" }
solana-account-decoder = { git = "https://github.com/rpcpool/solana-public.git", tag = "v3.0.6-triton-public" }
solana-account-decoder-client-types = { git = "https://github.com/rpcpool/solana-public.git", tag = "v3.0.6-triton-public" }
solana-client = { git = "https://github.com/rpcpool/solana-public.git", tag = "v3.0.6-triton-public" }
solana-streamer = { git = "https://github.com/rpcpool/solana-public.git", tag = "v3.0.6-triton-public" }
solana-transaction-context = { git = "https://github.com/rpcpool/solana-public.git", tag = "v3.0.6-triton-public" }
solana-transaction-status = { git = "https://github.com/rpcpool/solana-public.git", tag = "v3.0.6-triton-public" }
solana-transaction-status-client-types = { git = "https://github.com/rpcpool/solana-public.git", tag = "v3.0.6-triton-public" }
solana-net-utils = { git = "https://github.com/rpcpool/solana-public.git", tag = "v3.0.6-triton-public" }
solana-tpu-client = { git = "https://github.com/rpcpool/solana-public.git", tag = "v3.0.6-triton-public" }
solana-quic-client = { git = "https://github.com/rpcpool/solana-public.git", tag = "v3.0.6-triton-public" }
```

Example `example/Cargo.toml`:

```toml
[package]
name = "shield-tpu-example"
version = "0.1.0"
edition = "2021"

[dependencies]
yellowstone-jet-tpu-client = { workspace = true }
yellowstone-shield-store = { workspace = true }
yellowstone-grpc-client = { workspace = true }
solana-client = { workspace = true }
solana-keypair = { workspace = true }
solana-pubkey = { workspace = true }
solana-signature = { workspace = true }
solana-transaction = { workspace = true }
solana-message = { workspace = true }
solana-system-interface = { workspace = true }
solana-commitment-config = { workspace = true }
solana-signer = { workspace = true }
tokio = { workspace = true }
bincode = { workspace = true }
anyhow = { workspace = true }
```

How the pieces fit together:

```text
┌────────────────────────────────────────────────────────┐
│                  Your application                       │
│  1. Create a Shield PolicyStore                         │
│  2. Create a TPU sender                                 │
│  3. Build and sign a transaction                        │
│  4. Call send_txn_with_shield_policies()                │
└─────────────────┬───────────────────────────────────────┘
                  │
                  ▼
┌────────────────────────────────────────────────────────┐
│          Yellowstone TPU client (Jet)                   │
│  • Tracks the current slot                              │
│  • Knows the leader schedule                            │
│  • Checks: is the current leader allowed?               │
│    ├─ YES → send the transaction over QUIC              │
│    └─ NO  → skip (do not send)                          │
└────────────────────────────────────────────────────────┘
```

## Send with Shield

This complete example sends a SOL transfer only when the current leader passes your Shield policy. Fill in your own endpoints and x-token.

{% tabs %}
{% tab title="Rust" %}
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
    std::{str::FromStr, sync::Arc},
    yellowstone_jet_tpu_client::yellowstone_grpc::sender::{
        create_yellowstone_tpu_sender, Endpoints, ShieldBlockList,
    },
    yellowstone_shield_store::{
        PolicyStore, PolicyStoreConfig, PolicyStoreGrpcConfig, PolicyStoreRpcConfig,
    },
};

#[tokio::main]
async fn main() -> anyhow::Result<()> {
    let rpc_endpoint = "<your-rpc-endpoint>".to_string();
    let grpc_endpoint = "<your-grpc-endpoint>".to_string();
    let grpc_x_token = Some("<your-x-token>".to_string());

    // In production, read this from a file.
    let identity = Keypair::new();
    println!("Using identity: {}", identity.pubkey());

    let recipient = Pubkey::new_unique();

    // Step 1: set up the Shield policy store.
    let config = PolicyStoreConfig {
        rpc: PolicyStoreRpcConfig {
            endpoint: rpc_endpoint.clone(),
        },
        grpc: PolicyStoreGrpcConfig {
            endpoint: grpc_endpoint.clone(),
            x_token: grpc_x_token.clone(),
            commitment: None, // defaults to Confirmed
            timeout: std::time::Duration::from_secs(60),
            connect_timeout: std::time::Duration::from_secs(10),
            tcp_nodelay: true,
            http2_adaptive_window: true,
            http2_keep_alive: false,
            http2_keep_alive_interval: None,
            http2_keep_alive_timeout: None,
            http2_keep_alive_while_idle: None,
            max_decoding_message_size: Some(16 * 1024 * 1024), // 16 MiB
            initial_connection_window_size: None,
            initial_stream_window_size: None,
        },
    };

    let policy_store = PolicyStore::build().config(config).run().await?;

    // Use the Sandwiched.me MEV blocklist (DROPS).
    let shield_policy_pubkey =
        Pubkey::from_str("xMTozeQTEX2MR9KUon8vjg17U5Q9459RSASE3wy5eNB")?;

    // Step 2: create the TPU sender.
    let endpoints = Endpoints {
        rpc: rpc_endpoint.clone(),
        grpc: grpc_endpoint,
        grpc_x_token,
    };

    let tpu_sender_result =
        create_yellowstone_tpu_sender(Default::default(), identity.insecure_clone(), endpoints)
            .await?;

    let mut sender = tpu_sender_result.sender;
    let _related_objects_jh = tpu_sender_result.related_objects_jh;

    // Step 3: create the RPC client.
    let rpc_client = Arc::new(RpcClient::new_with_commitment(
        rpc_endpoint,
        CommitmentConfig::confirmed(),
    ));

    // Step 4: build the transaction.
    const LAMPORTS: u64 = 1000;
    let instructions = vec![transfer(&identity.pubkey(), &recipient, LAMPORTS)];

    let latest_blockhash = rpc_client.get_latest_blockhash().await?;

    let transaction = VersionedTransaction::try_new(
        VersionedMessage::V0(v0::Message::try_compile(
            &identity.pubkey(),
            &instructions,
            &[],
            latest_blockhash,
        )?),
        &[&identity],
    )?;

    let signature = transaction.signatures[0];
    let bincoded_txn = bincode::serialize(&transaction)?;

    // Step 5: send with Shield protection.
    let shield_blocklist = ShieldBlockList {
        policy_store: &policy_store,
        shield_policy_addresses: &[shield_policy_pubkey],
        default_return_value: true, // fail-open: send if the policy check itself fails
    };

    match sender
        .send_txn_with_shield_policies(signature, bincoded_txn, shield_blocklist)
        .await
    {
        Ok(_) => {
            println!("Transaction sent. Signature: {}", signature);
            println!("The current leader was not on the blocklist.");
        }
        Err(e) => {
            println!("Transaction not sent: {:?}", e);
            println!("The current leader was blocked by the Shield policy and the transaction was dropped.");
        }
    }

    Ok(())
}
```
{% endtab %}
{% endtabs %}

What the key pieces do:

* **`PolicyStoreConfig`** points the store at your RPC and gRPC endpoints.
* **`PolicyStore`** caches Shield policies from on-chain data and keeps them current over gRPC.
* **`ShieldBlockList`** links the policy store to the policy PDAs you want to enforce.
* **`send_txn_with_shield_policies()`** sends only when the current leader passes every policy.
* **`default_return_value: true`** is fail-open: if the policy check itself fails (for example a network issue), the transaction is sent anyway. Set it to `false` to fail closed.

## How it works under the hood

When you call `send_txn_with_shield_policies`, the client tracks the current slot, reads the current leader from the schedule, and checks that leader against your policies before deciding whether to send:

```text
1. send_txn_with_shield_policies(sig, txn, shield)
                  │
                  ▼
2. TPU client checks the current slot
                  │
                  ▼
3. Gets the current leader from the schedule
                  │
                  ▼
4. Shield check: is the leader allowed?
   ├─ PolicyStore → snapshot()
   └─ is_allowed([policies], &leader_pubkey)
                  │
        ┌─────────┴─────────┐
        ▼                   ▼
       YES                  NO
   5a. Send over QUIC   5b. Return SendError::RemotePeerBlocked
       to the leader        (transaction dropped)
```

The blocklist check, from the `yellowstone-jet` source:

{% tabs %}
{% tab title="Rust" %}
```rust
impl Blocklist for ShieldBlockList<'_> {
    fn is_blocked(&self, peer_address: &Pubkey) -> bool {
        // Blocked unless allowed by at least one policy.
        !self
            .policy_store
            .snapshot() // latest cached policy data
            .is_allowed(self.shield_policy_addresses, peer_address)
            .unwrap_or(self.default_return_value)
    }
}
```
{% endtab %}
{% endtabs %}

This means:

* **Blocklists (Deny):** if the validator is on the list, `is_blocked()` is true and the transaction is not sent.
* **Allowlists (Allow):** if the validator is not on the list, `is_blocked()` is true and the transaction is not sent.

The `PolicyStore` syncs policies continuously, so `snapshot()` is a fast, lock-free read:

{% tabs %}
{% tab title="Rust" %}
```rust
let policy_store = PolicyStore::build().config(config).run().await?;

// Later: read the current snapshot and check a validator.
let snapshot = policy_store.snapshot();
match snapshot.is_allowed(&[policy_pda], &validator_pubkey) {
    Ok(true) => println!("Validator allowed"),
    Ok(false) => println!("Validator blocked"),
    Err(e) => println!("Policy check failed: {:?}", e),
}
```
{% endtab %}
{% endtabs %}

## Handling blocked leaders

When a leader is blocked, the transaction is dropped, not queued. You have a few options:

**Retry with fanout** sends to the next few leaders instead:

{% tabs %}
{% tab title="Rust" %}
```rust
if sender.send_txn_with_shield_policies(sig, txn.clone(), shield).await.is_err() {
    // Current leader blocked, try the next 2 leaders.
    sender.send_txn_fanout_with_blocklist(sig, txn, 3, Some(shield)).await?;
}
```
{% endtab %}
{% endtabs %}

**Wait for the next slot** and try the new leader:

{% tabs %}
{% tab title="Rust" %}
```rust
tokio::time::sleep(std::time::Duration::from_millis(400)).await;
sender.send_txn_with_shield_policies(sig, txn, shield).await?;
```
{% endtab %}
{% endtabs %}

**Fail open** by setting `default_return_value: true`, so the transaction is sent even if every leader in the next few slots is blocked.

## Advanced usage

**Apply multiple policies.** The transaction is sent only if the leader passes all of them:

{% tabs %}
{% tab title="Rust" %}
```rust
let policies = vec![
    Pubkey::from_str("xMTozeQTEX2MR9KUon8vjg17U5Q9459RSASE3wy5eNB")?, // DROPS
    Pubkey::from_str("7xTAiL3tTzgbDwsxMKi2rMXW7QKzcGrABsZc2tW6L6bj")?, // SLOWAF
];

let shield_blocklist = ShieldBlockList {
    policy_store: &policy_store,
    shield_policy_addresses: &policies,
    default_return_value: true,
};

sender.send_txn_with_shield_policies(signature, txn, shield_blocklist).await?;
```
{% endtab %}
{% endtabs %}

**Broadcast to specific validators,** bypassing the leader schedule:

{% tabs %}
{% tab title="Rust" %}
```rust
let target_validators = vec![
    Pubkey::from_str("ValidatorA...")?,
    Pubkey::from_str("ValidatorB...")?,
];

sender.send_txn_many_dest(signature, bincoded_txn, &target_validators).await?;
```
{% endtab %}
{% endtabs %}

## Resources

* Shield program: [github.com/rpcpool/yellowstone-shield](https://github.com/rpcpool/yellowstone-shield)
* Jet engine: [github.com/rpcpool/yellowstone-jet](https://github.com/rpcpool/yellowstone-jet)
* Policy store crate: [`yellowstone-shield-store`](https://crates.io/crates/yellowstone-shield-store)
* Policy explorer: [validators.app/yellowstone-shield](https://www.validators.app/yellowstone-shield)
* Shield announcement: [Introducing Yellowstone Shield](https://blog.triton.one/introducing-yellowstone-shield)

## What's next

Pair Shield with a sender you control: the [Yellowstone Jet TPU client](yellowstone-jet-tpu-client.md) combines on-chain Shield policies with your own routing and retry logic.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
