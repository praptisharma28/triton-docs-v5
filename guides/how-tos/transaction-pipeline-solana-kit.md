---
description: >-
  Build, sign, send, and confirm a Solana transaction end to end with
  @solana/kit, walked through using Triton's Ping Thing client.
---

# End-to-end transaction pipeline with @solana/kit

This guide walks the Ping Thing client line by line to show how to perform common operations with the `@solana/kit` SDK (formerly `@solana/web3.js` 2). The Ping Thing is a collection of independent scripts that continuously send transactions to the chain to measure transaction landing time and slot latency.

It sends three types of transactions:

* SOL transfer
* Token transfer (USDC)
* Jupiter swaps (SOL to USDC)

Between them these cover some of the most common codepaths anyone hits when interacting with Solana. The walkthrough below uses the SOL transfer path. Source: [ping-thing-client on GitHub](https://github.com/Block-Logic/ping-thing-client).

{% hint style="info" %}
`@solana/kit` ships as independent packages to support modular design and tree shakeability. Import only what you use.
{% endhint %}

## Imports

```typescript
import {
  createTransactionMessage,
  pipe,
  setTransactionMessageFeePayer,
  setTransactionMessageLifetimeUsingBlockhash,
  createKeyPairFromBytes,
  getAddressFromPublicKey,
  createSignerFromKeyPair,
  signTransaction,
  appendTransactionMessageInstructions,
  sendTransactionWithoutConfirmingFactory,
  createSolanaRpcSubscriptions_UNSTABLE,
  getSignatureFromTransaction,
  compileTransaction,
  createSolanaRpc,
  SOLANA_ERROR__TRANSACTION_ERROR__BLOCKHASH_NOT_FOUND,
  isSolanaError,
  type Signature,
  type Commitment,
} from "@solana/kit";
import { getSetComputeUnitLimitInstruction, getSetComputeUnitPriceInstruction } from "@solana-program/compute-budget";
import { getTransferSolInstruction } from "@solana-program/system";
import { createRecentSignatureConfirmationPromiseFactory } from "@solana/transaction-confirmation";
```

## Build the pipeline

{% stepper %}
{% step %}
### Create RPC and websocket connections

Establish two connections: one for HTTP RPC calls and one for websocket subscriptions.

```typescript
// RPC connection for HTTP API calls, equivalent to `const c = new Connection(RPC_ENDPOINT)`
const rpcConnection = createSolanaRpc(RPC_ENDPOINT!);

// RPC connection for websocket connection
const rpcSubscriptions = createSolanaRpcSubscriptions_UNSTABLE(WS_ENDPOINT!);
```
{% endstep %}

{% step %}
### Load the keypair from the environment

Load the keypair the pinger uses to sign transactions from an environment variable.

```typescript
const USER_KEYPAIR = await createKeyPairFromBytes(
    bs58.decode(process.env.WALLET_PRIVATE_KEYPAIR!)
);
```
{% endstep %}

{% step %}
### Derive the fee-payer address and signer

The keypair gives you the fee-payer address and the signer for the transactions you create.

```typescript
const feePayer = await getAddressFromPublicKey(USER_KEYPAIR.publicKey);
const signer = await createSignerFromKeyPair(USER_KEYPAIR);
```
{% endstep %}

{% step %}
### Create the transaction

Build a SOL transfer transaction with `pipe`, setting version, fee payer, blockhash, and the instructions. The compute unit limit and price instructions set best-practice compute limits and a priority fee.

```typescript
const transaction = pipe(
  createTransactionMessage({ version: 0 }),
  (tx) => setTransactionMessageFeePayer(feePayer, tx),
  (tx) =>
    setTransactionMessageLifetimeUsingBlockhash(
      {
        blockhash: gBlockhash.value!,
        lastValidBlockHeight: BigInt(gBlockhash.lastValidBlockHeight!),
      },
      tx
    ),
  (tx) =>
    appendTransactionMessageInstructions(
      [
        getSetComputeUnitLimitInstruction({
          units: 500,
        }),
        getSetComputeUnitPriceInstruction({ microLamports: BigInt(PRIORITY_FEE_MICRO_LAMPORTS) }),

        // SOL transfer instruction
        getTransferSolInstruction({
          source: signer,
          destination: feePayer,
          amount: 5000,
        }),
      ],
      tx
    )
);
```

{% hint style="info" %}
The Ping Thing follows [best practices for transaction sending](https://docs.triton.one/chains/solana/sending-txs), including blockhash management and compute limits. See its [blockhash management source](https://github.com/Block-Logic/ping-thing-client/blob/main/src/utils/blockhash.rs).
{% endhint %}
{% endstep %}

{% step %}
### Sign the transaction

Compile the transaction message, sign it with the keypair, then read the signature off the signed transaction.

```typescript
// Sign the tx
const transactionSignedWithFeePayer = await signTransaction(
  [USER_KEYPAIR],
  compileTransaction(transaction)
);

// Get the tx signature
signature = getSignatureFromTransaction(transactionSignedWithFeePayer);
```
{% endstep %}

{% step %}
### Send the transaction

Create a sender via `sendTransactionWithoutConfirmingFactory`, then send. Factories let you assemble custom sending strategies.

```typescript
// Create a sender factory that sends a transaction and doesn't wait for confirmation
const mSendTransaction = sendTransactionWithoutConfirmingFactory({
  rpc: rpcConnection,
});

// Send the tx
await mSendTransaction(transactionSignedWithFeePayer, {
  commitment: "confirmed",
  maxRetries: 0n,
  skipPreflight: true,
});
```
{% endstep %}

{% step %}
### Confirm the transaction

Create a confirmation promise factory bound to both the RPC and the subscriptions connection, then await confirmation for the signature.

```typescript
// Create a promise factory that has the logic for the tx to be confirmed
const getRecentSignatureConfirmationPromise =
  createRecentSignatureConfirmationPromiseFactory({
    rpc: rpcConnection,
    rpcSubscriptions,
  });

await getRecentSignatureConfirmationPromise({
  signature,
  commitment: "confirmed",
  abortSignal: abortController.signal,
})
```
{% endstep %}
{% endstepper %}

## Fetching data over RPC

Data-fetching RPC calls use a builder that you finish with `.send()`. Always call `send()` after the method.

```typescript
const latestBlockhash = await connection.getLatestBlockhash().send()
```

## Subscribing to slot updates

Subscriptions are based on [`AsyncIterator`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/AsyncIterator). This creates a [`slotsUpdatesSubscribe`](https://solana.com/docs/rpc/websocket/slotsupdatessubscribe) subscription and updates a global slot value.

```typescript
// Subscribing to the `slotsUpdatesSubscribe` and update slot number
// https://solana.com/docs/rpc/websocket/slotsupdatessubscribe
const slotNotifications = await rpcSubscription
  .slotsUpdatesNotifications()
  .subscribe({ abortSignal: abortController.signal });

// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/AsyncIterator
for await (const notification of slotNotifications) {
  if (
    notification.type === "firstShredReceived" ||
    notification.type === "completed"
  ) {
    gSlotSent.value = notification.slot;
    gSlotSent.updated_at = Date.now();
    attempts = 0;
    continue;
  }
}
```

## Why everything is behind factories

Sending and confirming come from factory functions such as `sendTransactionWithoutConfirmingFactory` and `createRecentSignatureConfirmationPromiseFactory`. The reasons:

* Bind multiple RPC interfaces together so you do not pass them separately.
* Assemble the strategy (do x, y, z in a particular order).
* Create a closure that retains state spanning multiple invocations across your app.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
