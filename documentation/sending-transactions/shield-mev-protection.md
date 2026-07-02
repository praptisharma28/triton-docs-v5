---
description: Route Solana transactions only through validators you trust, using an on-chain policy.
---

# Shield MEV protection

Yellowstone Shield lets you control which validators handle your Solana transactions. You create an on-chain policy listing validators you trust (an allowlist) or do not trust (a blocklist), attach the policy when you send, and your transaction only goes to validators that match.

Solana's leader schedule is deterministic, so you often know which validator will process your transaction. If that leader runs sandwich attacks, frontrunning, or other harmful MEV, a normal send still routes to them. Shield is how you opt out. Background: [Introducing Yellowstone Shield](https://blog.triton.one/introducing-yellowstone-shield).

## Two ways to apply a policy

| Path                        | How                                                                                                                                                    | Use it when                                                            |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------- |
| **Triton RPC (SWQoS)**    | Add `forwardingPolicies` to your `sendTransaction` call. Triton checks the policy and routes for you.                                                  | You send through Triton RPC and want zero extra infrastructure.        |
| **Jet TPU client (direct)** | Call `send_txn_with_shield_policies` from the [Jet TPU client](jet-sender.md). Your code connects to validator TPUs and enforces the policy itself. | You run your own sender and need RPC-independent control over routing. |

Either way the policy lives on-chain and the enforcement is identical: if the current leader is not allowed, the transaction is dropped, not queued.

{% hint style="info" %}
Shield only works with Shield-enabled senders (Triton RPC, or the Jet TPU client). A standard Solana RPC ignores the policy and sends to every leader.
{% endhint %}

## Option A: Triton RPC (SWQoS)

Add the policy address to your `sendTransaction` request.

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sendTransaction",
  "params": [
    "<base64_encoded_transaction>",
    {
      "encoding": "base64",
      "skipPreflight": true,
      "forwardingPolicies": ["<your_policy_pda>"]
    }
  ]
}
```

Or set it as an HTTP header, comma-separated for multiple policies:

```
Solana-ForwardingPolicies: "<your_policy_pda>,<your_policy_pda2>"
```

## Option B: Jet TPU client (direct)

The [Jet TPU client](jet-sender.md) checks each leader against your policies inside your own process and sends only to allowed validators. Enable the `shield` feature and add `yellowstone-shield-store` (the workspace and `[patch.crates-io]` setup is on the [Jet TPU client](jet-sender.md) page).

Build a `PolicyStore` that caches the on-chain policies and live-updates them over gRPC, wrap your policy addresses in a `ShieldBlockList`, then send. Here `sender` is a TPU sender created as shown in the [Jet TPU client crate docs](https://docs.rs/yellowstone-jet-tpu-client).

```rust
use {
    solana_pubkey::Pubkey,
    std::{str::FromStr, time::Duration},
    yellowstone_jet_tpu_client::yellowstone_grpc::sender::ShieldBlockList,
    yellowstone_shield_store::{
        PolicyStore, PolicyStoreConfig, PolicyStoreGrpcConfig, PolicyStoreRpcConfig,
    },
};

// 1. Cache on-chain policies and keep them current over gRPC.
let config = PolicyStoreConfig {
    rpc: PolicyStoreRpcConfig { endpoint: rpc_endpoint.clone() },
    grpc: PolicyStoreGrpcConfig {
        endpoint: grpc_endpoint.clone(),
        x_token: grpc_x_token.clone(),
        commitment: None, // defaults to Confirmed
        timeout: Duration::from_secs(60),
        connect_timeout: Duration::from_secs(10),
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

// 2. A community blocklist of known MEV sandwichers (Sandwiched.me DROPS list).
let policy = Pubkey::from_str("xMTozeQTEX2MR9KUon8vjg17U5Q9459RSASE3wy5eNB")?;

// 3. Bind the policy store to your policy addresses.
let shield = ShieldBlockList {
    policy_store: &policy_store,
    shield_policy_addresses: &[policy],
    default_return_value: true, // fail open: send if the policy check itself errors
};

// 4. Send only if the current leader is allowed. Err means it was blocked and dropped.
sender
    .send_txn_with_shield_policies(signature, bincoded_txn, shield)
    .await?;
```

To try every leader instead of only the current one, `send_txn_fanout_with_blocklist` fans out across upcoming leaders with the same blocklist. For the complete program, see the [yellowstone-shield](https://github.com/rpcpool/yellowstone-shield) repository.

## Get a policy

### Use an existing one

Browse [validators.app/yellowstone-shield](https://www.validators.app/yellowstone-shield?locale=en\&network=mainnet) for community policies: deny lists of known sandwichers and poor performers, allow lists of top or geographically distributed validators. Copy the policy address and use it directly.

### Create your own

Install the CLI:

```bash
git clone https://github.com/rpcpool/yellowstone-shield
cd yellowstone-shield
cargo build --release --bin yellowstone-shield-cli
```

Create the policy. `--strategy deny` blocks the listed validators; `--strategy allow` permits only the listed validators. `--uri` points at policy metadata you have uploaded to IPFS or Arweave.

```bash
yellowstone-shield-cli policy create \
  --strategy deny \
  --name "My MEV blocklist" \
  --symbol "MEVBLOCK" \
  --uri "https://your-metadata-url.json"
# Output: Policy Mint Address: <your_policy_pda>
```

Add validators from a file with one identity per line:

```bash
yellowstone-shield-cli identities add \
  --mint <your_policy_pda> \
  --identities-path validators.txt
```

`identities update` replaces the whole list, `identities remove` drops specific validators, and `policy show --mint <pda>` prints the current policy.

## How policies and ownership work

Each policy is a Program Derived Address on-chain, paired with an SPL token (Token Extensions). Creating a policy mints you one token and keeps you the mint authority. Holding a token lets you manage the policy (add or remove validators); minting more lets you share management. Anyone who knows a policy's address can use it in their own transactions.

## Pricing

Shield adds no separate charge. On Triton RPC the `forwardingPolicies` enforcement is included on every send, so a Shield-protected `sendTransaction` is billed exactly like any other send.

## Limitations

Shield is a filter, not a guarantee.

* **Policies need active maintenance.** New validators join every epoch and a bad actor can appear at any time. A blocklist only protects against what is on it, so the quality of protection depends entirely on how current the list is kept.
* **Strict policies can drop time-sensitive transactions.** When no eligible leader is available the transaction is dropped, not queued. Be careful pairing a tight allowlist with arbitrage or liquidations.

## Resources

* Repository: [yellowstone-shield](https://github.com/rpcpool/yellowstone-shield)
* Policy explorer: [validators.app/yellowstone-shield](https://www.validators.app/yellowstone-shield?locale=en\&network=mainnet)
* Policy store crate: [yellowstone-shield-store](https://crates.io/crates/yellowstone-shield-store)
* Blog: [Introducing Yellowstone Shield](https://blog.triton.one/introducing-yellowstone-shield) and [Protect your transactions from MEV](https://blog.triton.one/protect-your-transactions-from-mev-with-yellowstone-shield-and-the-tpu-client)

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-play">:play:</i> <strong>Quickstart</strong></td><td>Send a transaction with /sendtx in a few minutes.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/quickstart">Quickstart</a></td></tr><tr><td><i class="fa-paper-plane">:paper-plane:</i> <strong>Jet sender</strong></td><td>The managed sending engine Shield policies run on.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/jet-sender">Jet sender</a></td></tr><tr><td><i class="fa-list-check">:list-check:</i> <strong>Best practices</strong></td><td>Land more transactions, faster.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/best-practices">Best practices</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
