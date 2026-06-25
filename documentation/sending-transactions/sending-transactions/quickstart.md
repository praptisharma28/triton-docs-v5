---
description: Send a Solana transaction with /sendtx, Triton's direct HTTP submission endpoint that skips JSON-RPC overhead.
layout:
  pagination:
    visible: false
---

# Quickstart

Send a Solana transaction with **`/sendtx`**, the direct HTTP submission endpoint on every Triton endpoint. Both `/sendtx` and `sendTransaction` route through Yellowstone Jet, Triton's sending engine, with SWQoS on by default; `/sendtx` just skips the JSON-RPC envelope: no JSON parsing, no CORS preflight, a smaller payload, and no RPC client library. For all the send options and how they compare, see [Sending transactions](https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions).

## 0. Prerequisites

* A Triton Solana endpoint and token, from your [customer dashboard](https://customers.triton.one).
* A funded keypair and a way to build and sign a transaction (`@solana/web3.js`, `@solana/kit`, or your SDK of choice).

## 1. Build and sign a transaction

Build and sign as usual, then serialise it to raw bytes. The signature is deterministic, so you can read it from the signed transaction before you send.

```ts
import {
  Connection, Keypair, PublicKey, SystemProgram, Transaction,
} from "@solana/web3.js";
import bs58 from "bs58";

const connection = new Connection("https://your-endpoint.mainnet.rpcpool.com/your-token", "confirmed");
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

## 2. Submit with /sendtx

POST the serialised transaction. Pass `response=signature` to get the signature back in the response body.

{% tabs %}
{% tab title="TypeScript" %}
```ts
const res = await fetch(
  "https://your-endpoint.mainnet.rpcpool.com/your-token/sendtx?response=signature&max_retries=3",
  {
    method: "POST",
    headers: { "Content-Type": "application/octet-stream" },
    body: wire,
  },
);

console.log("Signature:", await res.text());
```
{% endtab %}

{% tab title="curl (raw bytes)" %}
```bash
curl -X POST 'https://your-endpoint.mainnet.rpcpool.com/your-token/sendtx?response=signature&max_retries=3' \
  -H 'Content-Type: application/octet-stream' \
  --data-binary @transaction.bin
```
{% endtab %}

{% tab title="curl (base64)" %}
```bash
curl -X POST 'https://your-endpoint.mainnet.rpcpool.com/your-token/sendtx?encoding=base64&response=signature' \
  -H 'Content-Type: text/plain' \
  -d 'BASE64_SIGNED_TX'
```
{% endtab %}
{% endtabs %}

### Expected response

With `response=signature`, the body is the transaction signature as plain text, not JSON:

```
5j7s4Hk3vQmPq8nLZ9xTe8oP...
```

Query parameters: `encoding` (`base58` or `base64`, for text bodies; default `base58`), `response=signature` (return the signature on success), and `max_retries` (override the retry count). To route only through validators you trust, add the `Solana-ForwardingPolicies` header with your [Yellowstone Shield](https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/shield-mev-protection) policy addresses.

{% hint style="info" %}
Prefer the standard JSON-RPC interface, or need options like `skipPreflight`? `sendTransaction` works on the same endpoint. See [Best practices](https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/best-practices).
{% endhint %}

## 3. Verify

With `response=signature`, the POST returns the signature. Confirm it landed:

```bash
curl https://your-endpoint.mainnet.rpcpool.com/your-token -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getSignatureStatuses",
    "params": [["YOUR_SIGNATURE"], { "searchTransactionHistory": true }]
  }'
```

### Expected response

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

A `confirmationStatus` of `confirmed` or `finalized` means it landed.

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-paper-plane">:paper-plane:</i> <strong>Jet TPU client</strong></td><td>Full client-side control: send straight to validator TPUs over QUIC with callbacks and custom routing.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/jet-sender">Jet TPU client</a></td></tr><tr><td><i class="fa-arrow-trend-up">:arrow-trend-up:</i> <strong>Priority fees API</strong></td><td>Estimate a priority fee that lands under congestion.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/priority-fees-api">Priority fees API</a></td></tr><tr><td><i class="fa-list-check">:list-check:</i> <strong>Best practices</strong></td><td>Land transactions reliably under congestion.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/best-practices">Best practices</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
