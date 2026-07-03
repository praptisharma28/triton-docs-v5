---
description: Simulate Jito transaction bundles with Triton's Jito RPC support.
---

# Jito bundle simulation

Triton supports Jito's `simulateBundle` method on every plan, so you can test atomic transaction bundles on a Jito-enabled RPC before submitting them.

{% hint style="warning" %}
**Triton simulates Jito bundles, it does not forward them.** `simulateBundle` runs on Triton's Jito-enabled RPC nodes. Submitting a bundle for inclusion uses `sendBundle`, a Jito-exclusive call that needs a direct, active connection to the Jito Block Engine, so it does not route through Triton. To send bundles, connect to the Jito Block Engine yourself: your IP must be whitelisted with Jito to use those features.
{% endhint %}

## simulateBundle

Simulates a transaction bundle. Send it as a JSON-RPC `POST` to your Triton endpoint.

### Request body

| Name                      | Type   | Description                                                               |
| ------------------------- | ------ | ------------------------------------------------------------------------- |
| `transactions` (required) | array  | The list of transactions to simulate.                                     |
| `config`                  | object | Optional `RpcSimulateBundleConfig` controlling the simulation. See below. |

### Response

A JSON-RPC response matching this Rust struct:

```rust
pub struct RpcSimulateBundleResult {
    pub summary: RpcBundleSimulationSummary,
    pub transaction_results: Vec<RpcSimulateBundleTransactionResult>,
}

pub enum RpcBundleSimulationSummary {
    // Error and offending transaction signature, if applicable.
    Failed {
        error: RpcBundleExecutionError,
        tx_signature: Option<String>,
    },
    Succeeded,
}

pub struct RpcSimulateBundleTransactionResult {
    pub err: Option<TransactionError>,
    pub logs: Option<Vec<String>>,
    pub pre_execution_accounts: Option<Vec<UiAccount>>,
    pub post_execution_accounts: Option<Vec<UiAccount>>,
    pub units_consumed: Option<u64>,
    pub return_data: Option<UiTransactionReturnData>,
}
```

<details>

<summary>RpcBundleExecutionError variants</summary>

```rust
pub enum RpcBundleExecutionError {
    BankProcessingTimeLimitReached,         // hit the max allotted processing time
    BundleLockError,                        // a transaction is malformed
    BundleExecutionTimeout,                 // bundle execution timed out
    ExceedsCostModel,                       // the bundle exceeds the cost model
    InvalidPreOrPostAccounts,               // invalid pre or post accounts
    PohRecordError(String),
    TipError(String),
    TransactionFailure(Signature, String),  // a transaction failed to execute
}
```

</details>

<details>

<summary>RpcSimulateBundleConfig</summary>

```rust
pub struct RpcSimulateBundleConfig {
    // Pre/post account state. Length must equal the number of transactions.
    pub pre_execution_accounts_configs: Vec<Option<RpcSimulateTransactionAccountsConfig>>,
    pub post_execution_accounts_configs: Vec<Option<RpcSimulateTransactionAccountsConfig>>,
    // Encoding of the contained transactions.
    pub transaction_encoding: Option<UiTransactionEncoding>,
    // Which bank to simulate against.
    pub simulation_bank: Option<SimulationSlotConfig>,
    // Skip sig-verify for faster performance.
    pub skip_sig_verify: bool,
    // Replace the recent blockhash to simulate old transactions without resigning.
    pub replace_recent_blockhash: bool,
}
```

`simulation_bank` accepts a `Commitment`, a specific `Slot`, or `Tip` (the RPC's highest slot, the working bank).

</details>

## Resources

* [Jito searcher examples](https://github.com/jito-labs/searcher-examples)
* [Jito documentation: low-latency transactions](https://docs.jito.wtf/lowlatencytxnsend)

## Pricing

Simulating bundles on Triton's Jito-enabled RPC is billed as standard RPC (`$0.08 / GB` plus `$10 / million` calls). Submitting a bundle goes directly to the Jito Block Engine and is not billed by Triton.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
