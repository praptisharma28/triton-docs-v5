---
description: Send a Solana transaction two ways, with /sendtx, the direct HTTP submission endpoint, and with standard sendTransaction.
layout:
  pagination:
    visible: true
---

# Quickstart

In this quickstart we'll send a transaction two ways: **`/sendtx`**, the direct HTTP submission endpoint on every Triton endpoint, and standard **`sendTransaction`**. Both route through our Jet engine with SWQoS applied; `/sendtx` skips the JSON-RPC envelope for lower latency.

## 0. Prerequisites

* An active Triton subscription.
* Your endpoint URL and secret token from the [customer dashboard](https://customers.triton.one).
* A funded keypair and a way to build and sign a transaction (`@solana/web3.js`, `@solana/kit`, or your SDK of choice).

## 1. Build and sign a transaction

Build and sign as usual, then serialise it to raw bytes. The signature is deterministic, so you can read it from the signed transaction before you send.

```ts
import {
  Connection, Keypair, PublicKey, SystemProgram, Transaction,
} from "@solana/web3.js";
import bs58 from "bs58";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");
const payer = Keypair.fromSecretKey(Uint8Array.from(JSON.parse(process.env.SECRET_KEY!))); // 64-byte secret key

const tx = new Transaction().add(
  SystemProgram.transfer({
    fromPubkey: payer.publicKey,
    toPubkey: new PublicKey("RecipientAddressBase58"),
    lamports: 1000,
  }),
);
tx.recentBlockhash = (await connection.getLatestBlockhash()).blockhash;
tx.feePayer = payer.publicKey;
tx.sign(payer);

const wire = tx.serialize();           // raw bytes
const signature = bs58.encode(tx.signature!); // base58 signature, known before you send
```

## 2. Submit it

Two routes reach the same Jet delivery path. `/sendtx` is the lower-latency one:

* **No JSON parsing.** The server receives your transaction bytes directly.
* **No CORS preflight.** With `Content-Type: application/octet-stream` or `text/plain`, browsers skip the preflight `OPTIONS` round-trip.
* **Smaller payloads.** No `jsonrpc`/`id`/`method`/`params` wrapper on the wire.
* **Simpler client code.** One HTTP POST, no Solana JSON-RPC library.

That makes it the fit for browser apps sensitive to preflight latency and high-frequency backends sending volume. Pick `sendTransaction` when you want the standard JSON-RPC interface or options like `skipPreflight`.

{% tabs %}
{% tab title="/sendtx" %}
POST the serialised transaction. Pass `response=signature` to get the signature back in the response body.

{% tabs %}
{% tab title="TypeScript" %}
```ts
const res = await fetch(
  "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>/sendtx?response=signature&max_retries=3",
  {
    method: "POST",
    headers: { "Content-Type": "application/octet-stream" },
    body: new Uint8Array(wire),
  },
);

console.log("Signature:", await res.text());
```
{% endtab %}

{% tab title="curl (raw bytes)" %}
```bash
curl -X POST 'https://<your-endpoint>.mainnet.rpcpool.com/<your-token>/sendtx?response=signature&max_retries=3' \
  -H 'Content-Type: application/octet-stream' \
  --data-binary @transaction.bin
```
{% endtab %}

{% tab title="curl (base64)" %}
```bash
curl -X POST 'https://<your-endpoint>.mainnet.rpcpool.com/<your-token>/sendtx?encoding=base64&response=signature' \
  -H 'Content-Type: text/plain' \
  -d 'BASE64_SIGNED_TX'
```
{% endtab %}
{% endtabs %}

Query parameters: `encoding` (`base58` or `base64`, for text bodies; default `base58`), `response=signature` (return the signature on success), and `max_retries` (override the default retry count).

{% hint style="warning" %}
`/sendtx` is submission only and always skips preflight, so `skipPreflight: false` and simulation are not supported on it. Use `sendTransaction` if you need preflight, though we recommend running `simulateTransaction` as a separate call either way.
{% endhint %}

With `response=signature`, the response body is the transaction signature as plain text, not JSON:

```text
5j7s4Hk3vQmPq8nLZ9xTe8oP...
```
{% endtab %}

{% tab title="sendTransaction" %}
The standard Solana JSON-RPC method, on the same endpoint:

{% tabs %}
{% tab title="TypeScript" %}
```ts
const signature = await connection.sendRawTransaction(wire);
console.log("Signature:", signature);
```
{% endtab %}

{% tab title="curl" %}
```bash
curl https://<your-endpoint>.mainnet.rpcpool.com/<your-token> -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "sendTransaction",
    "params": ["BASE64_SIGNED_TX", { "encoding": "base64" }]
  }'
```
{% endtab %}
{% endtabs %}

The JSON-RPC result is the transaction signature:

```json
{ "jsonrpc": "2.0", "result": "5j7s4Hk3vQmPq8nLZ9xTe8oP...", "id": 1 }
```
{% endtab %}
{% endtabs %}

## 3. Verify it landed

With `response=signature`, the POST returns the signature. Confirm it landed:

{% tabs %}
{% tab title="curl" %}
```bash
curl https://<your-endpoint>.mainnet.rpcpool.com/<your-token> -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getSignatureStatuses",
    "params": [["YOUR_SIGNATURE"], { "searchTransactionHistory": true }]
  }'
```
{% endtab %}

{% tab title="Response" %}
The response shows the signature's confirmation status:

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": { "slot": 275123456 },
    "value": [
      { "slot": 275123456, "confirmations": 12, "err": null, "confirmationStatus": "confirmed" }
    ]
  },
  "id": 1
}
```
{% endtab %}
{% endtabs %}

A `confirmationStatus` of `confirmed` or `finalized` means it landed.

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-paper-plane">:paper-plane:</i> <strong>Jet TPU client</strong></td><td>End-to-end integration guide: direct-to-leader forwarding over QUIC with leader scheduling, connection pooling, and retries built in.</td><td><a href="https://app.gitbook.com/s/TpqU5Dqc6tdzY8J23dd7/solana/sending-transactions/yellowstone-jet-tpu-client">Jet TPU client</a></td></tr><tr><td><i class="fa-arrow-trend-up">:arrow-trend-up:</i> <strong>Priority Fees API</strong></td><td>Smart fee estimation with tail-aware percentiles. Reliable landing without overpaying.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/priority-fees-api">Priority Fees API</a></td></tr><tr><td><i class="fa-list-check">:list-check:</i> <strong>Best practices</strong></td><td>Land your transactions fast and reliably.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/best-practices">Best practices</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
