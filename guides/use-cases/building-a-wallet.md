---
description: >-
  Build a Solana wallet backend with Triton, covering balances, account-change
  streaming, history, and sending SOL and SPL tokens.
---

# Building a wallet

## Overview

This guide covers the core building blocks of a Solana wallet backend: fetching live balances, streaming real-time account changes, pulling transaction history, and sending SOL and SPL tokens reliably under network congestion.

By the end you'll have working code for each of these, plus a sending pattern that lands transactions consistently.

## Products used in this guide

| Product          | Role in this guide                                                   |
| ---------------- | -------------------------------------------------------------------- |
| Triton RPC       | Fetch SOL balance, token accounts, and transaction history           |
| Dragon's Mouth   | Subscribe to real-time account changes, no polling required          |
| Priority Fee API | Get a percentile-based fee estimate before sending                   |
| Jet              | Send transactions directly to the current leader with built-in SWQoS |

## Prerequisites

Before starting, make sure you have:

* A Triton account with an active endpoint.
* Your endpoint URL and token
* Basic familiarity with Solana (accounts, public keys, transactions)

{% tabs %}
{% tab title="TypeScript" %}
```bash
npm install @triton-one/yellowstone-grpc @solana/web3.js @solana/spl-token bs58
```
{% endtab %}

{% tab title="Rust" %}
```toml
# Add to Cargo.toml
[dependencies]
yellowstone-grpc-client = "13"
yellowstone-grpc-proto = "12"
tokio = { version = "1", features = ["full"] }
futures = "0.3"
bs58 = "0.5"
anyhow = "1"
reqwest = { version = "0.12", features = ["json"] }
serde = { version = "1", features = ["derive"] }
serde_json = "1"
solana-client = "3"
solana-sdk = "3"
solana-commitment-config = "3"
solana-compute-budget-interface = "3"
solana-system-interface = "2"
spl-token = "9"
spl-associated-token-account = "8"
```
{% endtab %}

{% tab title="Python" %}
```bash
pip install solana solders httpx grpcio grpcio-tools base58

# Generate yellowstone gRPC stubs from the proto files:
# python -m grpc_tools.protoc -I./proto --python_out=. --grpc_python_out=. geyser.proto
# Proto files: https://github.com/rpcpool/yellowstone-grpc/tree/master/yellowstone-grpc-proto/proto
```
{% endtab %}
{% endtabs %}

## Quickstart

Get your first response from Triton in under two minutes. Replace the endpoint and token, then run:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import { Connection } from "@solana/web3.js";

const connection = new Connection("https://your-endpoint.rpcpool.com/your-token");
const slot = await connection.getSlot();
console.log("Connected - current slot:", slot);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use solana_client::rpc_client::RpcClient;

fn main() {
    let client = RpcClient::new("https://your-endpoint.rpcpool.com/your-token");
    let slot = client.get_slot().unwrap();
    println!("Connected - current slot: {}", slot);
}
```
{% endtab %}

{% tab title="Python" %}
```python
from solana.rpc.api import Client

client = Client("https://your-endpoint.rpcpool.com/your-token")
slot = client.get_slot().value
print(f"Connected - current slot: {slot}")
```
{% endtab %}
{% endtabs %}

**What the output looks like:**

```text
Connected - current slot: 423835852
```

If you see a slot number, your endpoint is live and you're ready to go. The slot advances roughly every 400 ms, so if you run it twice you'll see it increment.

{% stepper %}
{% step %}
### Fetch wallet state

Get a wallet's SOL balance and all SPL token holdings in a single block of calls.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import {
  Connection,
  PublicKey,
  LAMPORTS_PER_SOL,
} from "@solana/web3.js";
import { TOKEN_PROGRAM_ID } from "@solana/spl-token";

const ENDPOINT = "https://your-endpoint.rpcpool.com";
const TOKEN = "your-token";

async function getWalletState(walletAddress: string) {
  const connection = new Connection(`${ENDPOINT}/${TOKEN}`);
  const pubkey = new PublicKey(walletAddress);

  // SOL balance
  const lamports = await connection.getBalance(pubkey);
  console.log(`SOL: ${lamports / LAMPORTS_PER_SOL}`);

  // All SPL token accounts owned by this wallet
  const { value: tokenAccounts } = await connection.getParsedTokenAccountsByOwner(
    pubkey,
    { programId: TOKEN_PROGRAM_ID }
  );

  for (const { account } of tokenAccounts) {
    const info = account.data.parsed.info;
    const mint: string = info.mint;
    const amount: number = info.tokenAmount.uiAmount;
    console.log(`Token ${mint}: ${amount}`);
  }
}

getWalletState("YourWalletAddressHere").catch(console.error);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use solana_client::rpc_client::RpcClient;
use solana_client::rpc_request::TokenAccountsFilter;
use solana_sdk::pubkey::Pubkey;
use std::str::FromStr;

const ENDPOINT: &str = "https://your-endpoint.rpcpool.com";
const TOKEN: &str = "your-token";
const TOKEN_PROGRAM_ID: &str = "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA";

fn get_wallet_state(wallet_address: &str) -> anyhow::Result<()> {
    let client = RpcClient::new(format!("{}/{}", ENDPOINT, TOKEN));
    let pubkey = Pubkey::from_str(wallet_address)?;

    // SOL balance
    let lamports = client.get_balance(&pubkey)?;
    println!("SOL: {:.9}", lamports as f64 / 1_000_000_000.0);

    // SPL token accounts - data comes back as parsed JSON with mint and amount
    let token_program = Pubkey::from_str(TOKEN_PROGRAM_ID)?;
    let accounts = client.get_token_accounts_by_owner(
        &pubkey,
        TokenAccountsFilter::ProgramId(token_program),
    )?;

    for account in &accounts {
        if let solana_client::rpc_response::UiAccountData::Json(ref parsed) = account.account.data {
            let info = &parsed.parsed["info"];
            let mint = info["mint"].as_str().unwrap_or("?");
            let balance = info["tokenAmount"]["uiAmountString"].as_str().unwrap_or("0");
            println!("  mint={} balance={}", mint, balance);
        }
    }

    Ok(())
}
```
{% endtab %}

{% tab title="Python" %}
```python
from solana.rpc.api import Client
from solana.rpc.types import TokenAccountOpts
from solders.pubkey import Pubkey

ENDPOINT = "https://your-endpoint.rpcpool.com"
TOKEN = "your-token"
TOKEN_PROGRAM_ID = "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"

def get_wallet_state(wallet_address: str):
    client = Client(f"{ENDPOINT}/{TOKEN}")
    pubkey = Pubkey.from_string(wallet_address)

    # SOL balance
    resp = client.get_balance(pubkey)
    print(f"SOL: {resp.value / 1_000_000_000:.9f}")

    # SPL token accounts
    opts = TokenAccountOpts(program_id=Pubkey.from_string(TOKEN_PROGRAM_ID))
    token_resp = client.get_token_accounts_by_owner_json_parsed(pubkey, opts)
    for account in token_resp.value:
        info = account.account.data.parsed["info"]
        mint = info["mint"]
        amount = info["tokenAmount"]["uiAmount"]
        print(f"Token {mint}: {amount}")
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Verify it works:** Replace `YourWalletAddressHere` with any active Solana address. You should see an SOL balance and a list of token mints with their amounts. An empty token list means the wallet has no SPL token accounts.
{% endhint %}

**What the output looks like:**

```text
SOL: 0.0
Token PhiLR4JDZB9z92rYT5xBXKCxmq4pGB1LYjtybii7aiS: 22100.0
Token kukb9fv4Y3MGuG1z6TodQqpctkYZjkGnuXQd91U8r1d: 28.0
Token 259n8rLH1y6J2DuLZFTUDGrpPW4WVF8fzJtwNQZyEDxF: 8.0
Token 2znw8JBTW1w2HBNiRwsTpbunjZVZ3uiGdHUt9M72i2oN: 9.0
Token HhzUT16NUeL93BHqQTTkCpzdRgxryxA9PP7aX2QNfg7B: 34312.0
```

Each line is a different SPL token the wallet holds. The address is the **mint**, the unique identifier for that token. `uiAmount` is already decimal-adjusted so you can display it directly.
{% endstep %}

{% step %}
### Stream real-time balance updates with Dragon's Mouth

Instead of polling on a timer, Dragon's Mouth pushes an update the moment an account changes, at confirmed commitment, typically within one slot of the transaction landing. Subscribe to your wallet's pubkey to get notified instantly when SOL is received or sent.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import Client, {
  CommitmentLevel,
  SubscribeRequest,
} from "@triton-one/yellowstone-grpc";
import bs58 from "bs58";

const ENDPOINT = "https://your-endpoint.rpcpool.com";
const TOKEN = "your-token";

async function streamWalletUpdates(walletAddress: string) {
  const client = new Client(ENDPOINT, TOKEN, {});
  await client.connect();
  const stream = await client.subscribe();

  const request: SubscribeRequest = {
    accounts: {
      wallet: {
        account: [walletAddress], // watch this specific account
        owner: [],
        filters: [],
      },
    },
    transactions: {},
    slots: {},
    transactionsStatus: {},
    entry: {},
    blocks: {},
    blocksMeta: {},
    commitment: CommitmentLevel.CONFIRMED,
    accountsDataSlice: [],
    ping: undefined,
  };

  stream.on("data", (data) => {
    if (data.account) {
      const pubkey = bs58.encode(data.account.account!.pubkey);
      const lamports = data.account.account!.lamports;
      const slot = data.account.slot;
      console.log(`Account updated: ${pubkey}`);
      console.log(`  New balance: ${Number(lamports) / 1e9} SOL`);
      console.log(`  Slot: ${slot}`);
    }
  });

  stream.on("error", (err) => console.error("Stream error:", err));

  await new Promise<void>((resolve, reject) => {
    stream.write(request, (err: Error | null) => {
      if (err) reject(err);
      else resolve();
    });
  });

  // Keep the stream alive
  setInterval(() => {
    stream.write({ ping: { id: 1 } } as SubscribeRequest, () => {});
  }, 30_000);
}

streamWalletUpdates("YourWalletAddressHere").catch(console.error);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use std::collections::HashMap;
use futures::StreamExt;
use yellowstone_grpc_client::GeyserGrpcClient;
use yellowstone_grpc_proto::prelude::{
    subscribe_update::UpdateOneof, CommitmentLevel, SubscribeRequest,
    SubscribeRequestFilterAccounts,
};

const ENDPOINT: &str = "https://your-endpoint.rpcpool.com";
const TOKEN: &str = "your-token";

#[tokio::main]
async fn main() -> anyhow::Result<()> {
    let wallet_address = "YourWalletAddressHere";

    let mut client = GeyserGrpcClient::build_from_shared(ENDPOINT.to_string())?
        .x_token(Some(TOKEN.to_string()))?
        .connect()
        .await?;

    let mut accounts = HashMap::new();
    accounts.insert(
        "wallet".to_string(),
        SubscribeRequestFilterAccounts {
            account: vec![wallet_address.to_string()],
            ..Default::default()
        },
    );

    let request = SubscribeRequest {
        accounts,
        commitment: Some(CommitmentLevel::Confirmed as i32),
        ..Default::default()
    };

    let (_tx, mut stream) = client.subscribe_with_request(Some(request)).await?;

    while let Some(Ok(msg)) = stream.next().await {
        if let Some(UpdateOneof::Account(acct)) = msg.update_oneof {
            let lamports = acct.account.as_ref().map(|a| a.lamports).unwrap_or(0);
            let pubkey = bs58::encode(
                acct.account.as_ref().map(|a| a.pubkey.as_slice()).unwrap_or(&[])
            ).into_string();
            println!("Account updated: {}", pubkey);
            println!("  New balance: {:.9} SOL", lamports as f64 / 1e9);
            println!("  Slot: {}", acct.slot);
        }
    }

    Ok(())
}
```
{% endtab %}

{% tab title="Python" %}
```python
import asyncio
import grpc
from grpc import aio
import base58

from geyser_pb2 import (
    SubscribeRequest,
    SubscribeRequestFilterAccounts,
    CommitmentLevel,
    SubscribeRequestPing,
)
from geyser_pb2_grpc import GeyserStub

ENDPOINT = "your-endpoint.rpcpool.com:443"
TOKEN = "your-token"

async def stream_wallet_updates(wallet_address: str):
    credentials = grpc.ssl_channel_credentials()
    async with aio.secure_channel(ENDPOINT, credentials) as channel:
        stub = GeyserStub(channel)
        metadata = (("x-token", TOKEN),)

        async def request_iterator():
            yield SubscribeRequest(
                accounts={
                    "wallet": SubscribeRequestFilterAccounts(
                        account=[wallet_address],
                    )
                },
                commitment=CommitmentLevel.CONFIRMED,
            )
            while True:
                await asyncio.sleep(30)
                yield SubscribeRequest(ping=SubscribeRequestPing(id=1))

        async for update in stub.Subscribe(request_iterator(), metadata=metadata):
            if update.HasField("account"):
                pubkey = base58.b58encode(bytes(update.account.account.pubkey)).decode()
                lamports = update.account.account.lamports
                slot = update.account.slot
                print(f"Account updated: {pubkey}")
                print(f"  New balance: {lamports / 1e9:.9f} SOL")
                print(f"  Slot: {slot}")

asyncio.run(stream_wallet_updates("YourWalletAddressHere"))
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Verify it works:** As soon as the subscription connects, you'll receive the current account state immediately. You'll then see an update within milliseconds whenever the account changes.
{% endhint %}

**What the output looks like:**

```text
Account updated: 58oQChx4yWmvKdwLLZzBi4ChoCc2fqCUWBkwMihLYQo2
  New balance: 4.781558248 SOL
  Slot: 423839570
Account updated: 58oQChx4yWmvKdwLLZzBi4ChoCc2fqCUWBkwMihLYQo2
  New balance: 4.781558248 SOL
  Slot: 423839571
```

Updates arrive approximately every slot (\~400 ms) for active accounts. The `filters` field in the raw stream confirms which subscription key matched, here `"wallet"`, matching the key we set in the request.

**Watching token accounts:** The same subscription works for SPL token accounts. Add each token account pubkey to the `account` filter. When a token transfer happens, the account's `data` field changes. Decode it with the SPL Token account layout to read the updated balance:

```typescript
import { AccountLayout } from "@solana/spl-token";

stream.on("data", (data) => {
  if (data.account) {
    const raw = Buffer.from(data.account.account!.data);
    if (raw.length === AccountLayout.span) {
      const decoded = AccountLayout.decode(raw);
      // decoded.amount is a BigInt in atomic units
      // divide by 10 ** decimals to get the display amount
      console.log(`Token balance (atomic): ${decoded.amount}`);
    }
  }
});
```
{% endstep %}

{% step %}
### Fetch transaction history

Pull a wallet's recent transactions with `getSignaturesForAddress`, then fetch the full details of each.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import {
  Connection,
  PublicKey,
  ParsedTransactionWithMeta,
} from "@solana/web3.js";

const ENDPOINT = "https://your-endpoint.rpcpool.com";
const TOKEN = "your-token";

async function getTransactionHistory(
  walletAddress: string,
  limit: number = 10
): Promise<ParsedTransactionWithMeta[]> {
  const connection = new Connection(`${ENDPOINT}/${TOKEN}`);
  const pubkey = new PublicKey(walletAddress);

  // Most recent signatures first
  const signatures = await connection.getSignaturesForAddress(pubkey, { limit });
  console.log(`Found ${signatures.length} transactions`);

  // Fetch full parsed transaction details
  const transactions = await connection.getParsedTransactions(
    signatures.map((s) => s.signature),
    { maxSupportedTransactionVersion: 0 }
  );

  for (const tx of transactions) {
    if (!tx) continue;
    const sig = tx.transaction.signatures[0];
    const slot = tx.slot;
    const status = tx.meta?.err ? "failed" : "ok";
    const fee = tx.meta?.fee ?? 0;
    console.log(`${sig.slice(0, 20)}... slot=${slot} status=${status} fee=${fee}`);
  }

  return transactions.filter(Boolean) as ParsedTransactionWithMeta[];
}

getTransactionHistory("YourWalletAddressHere").catch(console.error);
```
{% endtab %}

{% tab title="Rust" %}
```rust
// solana-sdk 3.x: CommitmentConfig moved to solana-commitment-config crate;
// the config struct is GetConfirmedSignaturesForAddress2Config (not RpcSignaturesForAddressConfig);
// sig_info.slot is u64 (not Option<u64>).
use solana_client::rpc_client::{GetConfirmedSignaturesForAddress2Config, RpcClient};
use solana_commitment_config::CommitmentConfig;
use solana_sdk::pubkey::Pubkey;
use std::str::FromStr;

const ENDPOINT: &str = "https://your-endpoint.rpcpool.com";
const TOKEN: &str = "your-token";

fn get_transaction_history(wallet_address: &str, limit: usize) -> anyhow::Result<()> {
    let client = RpcClient::new(format!("{}/{}", ENDPOINT, TOKEN));
    let pubkey = Pubkey::from_str(wallet_address)?;

    let config = GetConfirmedSignaturesForAddress2Config {
        limit: Some(limit),
        commitment: Some(CommitmentConfig::finalized()),
        ..Default::default()
    };

    let sigs = client.get_signatures_for_address_with_config(&pubkey, config)?;
    println!("Found {} transactions", sigs.len());

    for sig_info in &sigs {
        let status = if sig_info.err.is_some() { "failed" } else { "ok" };
        println!(
            "{:.20}... slot={} status={}",
            sig_info.signature,
            sig_info.slot,
            status
        );
    }

    Ok(())
}
```
{% endtab %}

{% tab title="Python" %}
```python
from solana.rpc.api import Client
from solders.pubkey import Pubkey

ENDPOINT = "https://your-endpoint.rpcpool.com"
TOKEN = "your-token"

def get_transaction_history(wallet_address: str, limit: int = 10):
    client = Client(f"{ENDPOINT}/{TOKEN}")
    pubkey = Pubkey.from_string(wallet_address)

    sigs = client.get_signatures_for_address(pubkey, limit=limit)
    print(f"Found {len(sigs.value)} transactions")

    for sig_info in sigs.value:
        status = "failed" if sig_info.err else "ok"
        print(f"{str(sig_info.signature)[:20]}... slot={sig_info.slot} status={status}")
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Verify it works:** Use any active Solana wallet address with transaction history. You should see up to 10 recent signatures with their slot and status.
{% endhint %}

**What the output looks like:**

```text
Found 5 transactions
4x7M5EjrJkuheoRmQCGR... slot=408661772 status=ok
5Be9V7hfrimVyjHz15kb... slot=377756257 status=ok
4KRoLyKGv4sWFDdwtmAj... slot=377749681 status=ok
5PUkggk1qC3KiobK2QHk... slot=377749346 status=ok
qmLhqJhUr4nzv72PMr7D... slot=364454544 status=ok
```

**Paginating further back:** Pass `before` to walk back through history:

```typescript
const olderSigs = await connection.getSignaturesForAddress(pubkey, {
  limit: 10,
  before: signatures[signatures.length - 1].signature,
});
```
{% endstep %}

{% step %}
### Send SOL

Build a SOL transfer, attach a priority fee from the Priority Fee API, and send via Jet.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import {
  Connection,
  PublicKey,
  SystemProgram,
  Transaction,
  ComputeBudgetProgram,
  Keypair,
  LAMPORTS_PER_SOL,
} from "@solana/web3.js";

const ENDPOINT = "https://your-endpoint.rpcpool.com";
const TOKEN = "your-token";

async function sendSol(
  sender: Keypair,
  recipientAddress: string,
  amountSol: number
) {
  const connection = new Connection(`${ENDPOINT}/${TOKEN}`);
  const recipient = new PublicKey(recipientAddress);
  const lamports = Math.round(amountSol * LAMPORTS_PER_SOL);

  // 1. Get a competitive priority fee (90th percentile)
  const feeResponse = await fetch(`${ENDPOINT}/${TOKEN}`, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      jsonrpc: "2.0",
      id: 1,
      method: "getRecentPrioritizationFees",
      params: [[sender.publicKey.toBase58()], { percentile: 9000 }],
    }),
  });
  const { result } = await feeResponse.json();
  const priorityFee: number = result.sort(
    (a: { slot: number }, b: { slot: number }) => b.slot - a.slot
  )[0].prioritizationFee;

  // 2. Build the transaction
  const { blockhash } = await connection.getLatestBlockhash();
  const tx = new Transaction();
  tx.recentBlockhash = blockhash;
  tx.feePayer = sender.publicKey;

  tx.add(
    ComputeBudgetProgram.setComputeUnitLimit({ units: 200_000 }),
    ComputeBudgetProgram.setComputeUnitPrice({ microLamports: priorityFee }),
    SystemProgram.transfer({
      fromPubkey: sender.publicKey,
      toPubkey: recipient,
      lamports,
    })
  );

  tx.sign(sender);

  // 3. Send via Jet - skipPreflight + maxRetries: 0 for direct leader delivery
  const sendResponse = await fetch(`${ENDPOINT}/${TOKEN}`, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      jsonrpc: "2.0",
      id: 1,
      method: "sendTransaction",
      params: [
        tx.serialize().toString("base64"),
        { encoding: "base64", skipPreflight: true, maxRetries: 0 },
      ],
    }),
  });

  const { result: signature } = await sendResponse.json();
  console.log("Sent:", signature);
  return signature as string;
}
```
{% endtab %}

{% tab title="Rust" %}
```rust
// solana-sdk 3.x: ComputeBudgetInstruction and system_instruction moved to separate crates.
// Use Keypair::from_base58_string (from_bytes removed in 3.x).
// The percentile param works on Triton endpoints; public devnet RPC ignores it.
use anyhow::anyhow;
use reqwest::Client as HttpClient;
use serde::Deserialize;
use solana_client::rpc_client::RpcClient;
use solana_client::rpc_config::RpcSendTransactionConfig;
use solana_compute_budget_interface::ComputeBudgetInstruction;
use solana_sdk::{
    pubkey::Pubkey,
    signature::{Keypair, Signer},
    transaction::Transaction,
};
use solana_system_interface::instruction as system_instruction;
use std::str::FromStr;

const ENDPOINT: &str = "https://your-endpoint.rpcpool.com";
const TOKEN: &str = "your-token";

#[derive(Deserialize)]
struct PrioritizationFee {
    slot: u64,
    #[serde(rename = "prioritizationFee")]
    prioritization_fee: u64,
}

#[derive(Deserialize)]
struct RpcResponse {
    result: Vec<PrioritizationFee>,
}

async fn get_priority_fee(accounts: &[&str], percentile: u32) -> anyhow::Result<u64> {
    let http = HttpClient::new();
    let url = format!("{}/{}", ENDPOINT, TOKEN);
    let body = serde_json::json!({
        "jsonrpc": "2.0", "id": 1,
        "method": "getRecentPrioritizationFees",
        "params": [accounts, { "percentile": percentile }]
    });
    let mut resp: RpcResponse = http.post(&url).json(&body).send().await?.json().await?;
    resp.result.sort_by(|a, b| b.slot.cmp(&a.slot));
    resp.result.first().map(|f| f.prioritization_fee).ok_or_else(|| anyhow!("no fee data"))
}

async fn send_sol(sender: &Keypair, recipient_address: &str, lamports: u64) -> anyhow::Result<String> {
    let client = RpcClient::new(format!("{}/{}", ENDPOINT, TOKEN));
    let recipient = Pubkey::from_str(recipient_address)?;

    let priority_fee = get_priority_fee(&[&sender.pubkey().to_string()], 9000).await?;
    let blockhash = client.get_latest_blockhash()?;

    let tx = Transaction::new_signed_with_payer(
        &[
            ComputeBudgetInstruction::set_compute_unit_limit(200_000),
            ComputeBudgetInstruction::set_compute_unit_price(priority_fee),
            system_instruction::transfer(&sender.pubkey(), &recipient, lamports),
        ],
        Some(&sender.pubkey()),
        &[sender],
        blockhash,
    );

    let config = RpcSendTransactionConfig {
        skip_preflight: true,
        max_retries: Some(0),
        ..Default::default()
    };

    let sig = client.send_transaction_with_config(&tx, config)?;
    println!("Sent: {}", sig);
    Ok(sig.to_string())
}
```
{% endtab %}

{% tab title="Python" %}
```python
import httpx
from solana.rpc.api import Client
from solana.rpc.types import TxOpts
from solders.keypair import Keypair
from solders.pubkey import Pubkey
from solders.system_program import TransferParams, transfer
from solders.transaction import Transaction as SolTransaction
from solders.message import Message
from solders.compute_budget import set_compute_unit_price, set_compute_unit_limit

ENDPOINT = "https://your-endpoint.rpcpool.com"
TOKEN = "your-token"

async def get_priority_fee(accounts: list[str], percentile: int) -> int:
    async with httpx.AsyncClient() as http:
        resp = await http.post(
            f"{ENDPOINT}/{TOKEN}",
            json={
                "jsonrpc": "2.0", "id": 1,
                "method": "getRecentPrioritizationFees",
                "params": [accounts, {"percentile": percentile}],
            },
        )
        fees = resp.json()["result"]
        fees.sort(key=lambda x: x["slot"], reverse=True)
        return fees[0]["prioritizationFee"]

def send_sol(sender: Keypair, recipient_address: str, lamports: int, priority_fee: int):
    client = Client(f"{ENDPOINT}/{TOKEN}")
    recipient = Pubkey.from_string(recipient_address)

    blockhash = client.get_latest_blockhash().value.blockhash

    instructions = [
        set_compute_unit_limit(200_000),
        set_compute_unit_price(priority_fee),
        transfer(TransferParams(
            from_pubkey=sender.pubkey(),
            to_pubkey=recipient,
            lamports=lamports,
        )),
    ]

    msg = Message.new_with_blockhash(instructions, sender.pubkey(), blockhash)
    tx = SolTransaction([sender], msg, blockhash)

    response = client.send_raw_transaction(
        bytes(tx),
        opts=TxOpts(skip_preflight=True, max_retries=0),
    )
    print(f"Sent: {response.value}")
    return str(response.value)
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**`skipPreflight: true` and `maxRetries: 0`** are required when sending via Jet. With retries disabled, Jet takes ownership of delivering the transaction directly to the current and upcoming leaders using SWQoS, giving you better landing rates than the default RPC retry behaviour.
{% endhint %}

{% hint style="info" %}
**Verify it works:** Run with a funded keypair and a valid recipient. You should see a signature printed immediately. Confirm it landed with:

```typescript
const status = await connection.getSignatureStatus(signature);
console.log(status.value?.confirmationStatus); // "confirmed" or "finalized"
```
{% endhint %}

**What the output looks like:**

The priority fee call returns the current network fee before the transaction is built:

```text
Priority fee (90th pct): 2740943 micro-lamports/CU
```

Then the send returns a signature:

```text
Sent: 5ofMHS3yeHrLReLkij5V9SvcrTZoKajJYaLw8aos2UeM2cupWtARXp1Jrt9WsAKTCcqPbja2vQv8nyxmcurcvwg1
Status: confirmed ✅
```

The response is a base58-encoded transaction signature (88 characters). This is your receipt, use it to look up the transaction in an explorer or poll `getSignatureStatus` to confirm it landed.
{% endstep %}

{% step %}
### Send SPL tokens

The pattern is the same as sending SOL: build a transaction with a priority fee and send via Jet. The instruction is `createTransferCheckedInstruction` from `@solana/spl-token`.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import {
  Connection,
  PublicKey,
  Transaction,
  ComputeBudgetProgram,
  Keypair,
} from "@solana/web3.js";
import {
  getOrCreateAssociatedTokenAccount,
  createTransferCheckedInstruction,
  getMint,
} from "@solana/spl-token";

const ENDPOINT = "https://your-endpoint.rpcpool.com";
const TOKEN = "your-token";

async function sendToken(
  sender: Keypair,
  recipientAddress: string,
  mintAddress: string,
  amount: number,      // in display units, e.g. 1.5 for 1.5 USDC
  priorityFee: number  // micro-lamports/CU from Priority Fee API
) {
  const connection = new Connection(`${ENDPOINT}/${TOKEN}`);
  const recipient = new PublicKey(recipientAddress);
  const mint = new PublicKey(mintAddress);

  // Get mint decimals
  const mintInfo = await getMint(connection, mint);
  const atomicAmount = BigInt(Math.round(amount * 10 ** mintInfo.decimals));

  // Get (or create) associated token accounts for sender and recipient.
  // Pass "confirmed" so the post-creation read uses confirmed finality -
  // without it, the re-fetch can fail on load-balanced RPCs if the creating
  // node and reading node haven't fully synced.
  const senderAta = await getOrCreateAssociatedTokenAccount(
    connection, sender, mint, sender.publicKey, false, "confirmed"
  );
  const recipientAta = await getOrCreateAssociatedTokenAccount(
    connection,
    sender,    // payer for ATA creation if the recipient doesn't have one yet
    mint,
    recipient,
    false,
    "confirmed"
  );

  const { blockhash } = await connection.getLatestBlockhash();
  const tx = new Transaction();
  tx.recentBlockhash = blockhash;
  tx.feePayer = sender.publicKey;

  tx.add(
    ComputeBudgetProgram.setComputeUnitLimit({ units: 300_000 }),
    ComputeBudgetProgram.setComputeUnitPrice({ microLamports: priorityFee }),
    createTransferCheckedInstruction(
      senderAta.address,
      mint,
      recipientAta.address,
      sender.publicKey,
      atomicAmount,
      mintInfo.decimals
    )
  );

  tx.sign(sender);

  const sendResponse = await fetch(`${ENDPOINT}/${TOKEN}`, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      jsonrpc: "2.0",
      id: 1,
      method: "sendTransaction",
      params: [
        tx.serialize().toString("base64"),
        { encoding: "base64", skipPreflight: true, maxRetries: 0 },
      ],
    }),
  });

  const { result: signature } = await sendResponse.json();
  console.log("Sent:", signature);
  return signature as string;
}
```
{% endtab %}

{% tab title="Rust" %}
```rust
// Note: source and destination ATAs must already exist.
// Use spl-associated-token-account = "8" to derive or create them if needed.
// solana-sdk 3.x: ComputeBudgetInstruction moved to solana-compute-budget-interface crate.
// spl-token = "9" is required (v6 uses solana-program 2.x which conflicts with solana-sdk 3.x).
use solana_client::rpc_client::RpcClient;
use solana_client::rpc_config::RpcSendTransactionConfig;
use solana_compute_budget_interface::ComputeBudgetInstruction;
use solana_sdk::{
    pubkey::Pubkey,
    signature::{Keypair, Signer},
    transaction::Transaction,
};
use spl_token::instruction::transfer_checked;

const ENDPOINT: &str = "https://your-endpoint.rpcpool.com";
const TOKEN: &str = "your-token";

fn send_token(
    sender: &Keypair,
    source_ata: &Pubkey,
    dest_ata: &Pubkey,
    mint: &Pubkey,
    amount: u64,      // atomic units
    decimals: u8,
    priority_fee: u64,
) -> anyhow::Result<String> {
    let client = RpcClient::new(format!("{}/{}", ENDPOINT, TOKEN));
    let blockhash = client.get_latest_blockhash()?;

    let transfer_ix = transfer_checked(
        &spl_token::id(),
        source_ata,
        mint,
        dest_ata,
        &sender.pubkey(),
        &[],
        amount,
        decimals,
    )?;

    let tx = Transaction::new_signed_with_payer(
        &[
            ComputeBudgetInstruction::set_compute_unit_limit(300_000),
            ComputeBudgetInstruction::set_compute_unit_price(priority_fee),
            transfer_ix,
        ],
        Some(&sender.pubkey()),
        &[sender],
        blockhash,
    );

    let config = RpcSendTransactionConfig {
        skip_preflight: true,
        max_retries: Some(0),
        ..Default::default()
    };

    let sig = client.send_transaction_with_config(&tx, config)?;
    println!("Sent: {}", sig);
    Ok(sig.to_string())
}
```
{% endtab %}

{% tab title="Python" %}
```python
# spl.token.instructions is included in `pip install solana` - no separate package needed.
from solana.rpc.api import Client
from solana.rpc.types import TxOpts
from solders.keypair import Keypair
from solders.pubkey import Pubkey
from solders.transaction import Transaction as SolTransaction
from solders.message import Message
from solders.compute_budget import set_compute_unit_price, set_compute_unit_limit
from spl.token.instructions import transfer_checked, TransferCheckedParams
from spl.token.constants import TOKEN_PROGRAM_ID

ENDPOINT = "https://your-endpoint.rpcpool.com"
TOKEN = "your-token"

def send_token(
    sender: Keypair,
    source_ata: Pubkey,
    dest_ata: Pubkey,
    mint: Pubkey,
    amount: int,      # atomic units
    decimals: int,
    priority_fee: int,
):
    client = Client(f"{ENDPOINT}/{TOKEN}")
    blockhash = client.get_latest_blockhash().value.blockhash

    instructions = [
        set_compute_unit_limit(300_000),
        set_compute_unit_price(priority_fee),
        transfer_checked(TransferCheckedParams(
            program_id=TOKEN_PROGRAM_ID,
            source=source_ata,
            mint=mint,
            dest=dest_ata,
            owner=sender.pubkey(),
            amount=amount,
            decimals=decimals,
            signers=[],
        )),
    ]

    msg = Message.new_with_blockhash(instructions, sender.pubkey(), blockhash)
    tx = SolTransaction([sender], msg, blockhash)

    response = client.send_raw_transaction(
        bytes(tx),
        opts=TxOpts(skip_preflight=True, max_retries=0),
    )
    print(f"Sent: {response.value}")
    return str(response.value)
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**`getOrCreateAssociatedTokenAccount`** will create the recipient's associated token account if it doesn't exist yet, funded by the sender. To avoid paying for ATA creation, check first with `getAssociatedTokenAddress` and fail early if the account doesn't exist:

```typescript
import { getAssociatedTokenAddress, getAccount } from "@solana/spl-token";

const recipientAta = await getAssociatedTokenAddress(mint, recipient);
try {
  await getAccount(connection, recipientAta); // throws if not found
} catch {
  throw new Error("Recipient has no token account for this mint");
}
```
{% endhint %}

{% hint style="info" %}
**Verify it works:** Run with a funded wallet holding some SPL tokens. You should see a signature. If you get a `TokenAccountNotFoundError` after `getOrCreateAssociatedTokenAccount` creates the ATA, the read-back hit a different load-balanced node before it propagated. The `"confirmed"` commitment above prevents this.
{% endhint %}

**What the output looks like:**

```text
Sent: 4M43KqiKonCP86oqmmb6...........................................
Status: confirmed ✅

Sender balance after:    90 tokens
Recipient balance after: 10 tokens
```

The token balances update atomically: if the transaction lands, the full amount moved. Check `preTokenBalances` vs `postTokenBalances` in the confirmed transaction to verify the exact amount transferred.
{% endstep %}
{% endstepper %}

## What's next

* **Routing and swaps:** swap any SPL token pair through Triton's hosted Metis (Jupiter routing) or Titan (RFQ aggregator). Coming soon.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
