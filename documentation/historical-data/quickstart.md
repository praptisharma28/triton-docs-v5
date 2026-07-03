---
description: Pull a Solana address's full transaction history, back to genesis, in one Superbank request.
layout:
  pagination:
    visible: true
---

# Quickstart

Read Solana's complete on-chain history on Triton. Historical methods are served from Superbank automatically, as spec-compliant Solana JSON-RPC, so the calls you already know work unchanged across the full ledger back to genesis (billed at $0.08/GB plus $10 per million requests). This reads an address's history two ways: the standard `getSignaturesForAddress` then `getTransaction` pattern, and the `getTransactionsForAddress` extension that returns it in a single call.

## 0. Prerequisites

* A Triton Solana endpoint and token with historical data enabled, from your [customer dashboard](https://customers.triton.one).
* `curl` or any Solana JSON-RPC client. The examples use the USDC mint (`EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v`); swap in any address.

## 1. List an address's signatures

`getSignaturesForAddress` returns the transaction signatures that touch an address, newest first.

{% tabs %}
{% tab title="curl" %}
```bash
curl https://<your-endpoint>.mainnet.rpcpool.com/<your-token> -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getSignaturesForAddress",
    "params": ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v", { "limit": 5 }]
  }'
```
{% endtab %}

{% tab title="TypeScript" %}
```typescript
const res = await fetch("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    jsonrpc: "2.0",
    id: 1,
    method: "getSignaturesForAddress",
    params: ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v", { limit: 5 }],
  }),
});
console.log((await res.json()).result);
```
{% endtab %}
{% endtabs %}

### Expected response

An array of signature objects, newest first:

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "signature": "5wHu1qwD4kLpXnR7e8oP...",
      "slot": 250000123,
      "blockTime": 1700000000,
      "confirmationStatus": "finalized",
      "err": null,
      "memo": null
    }
  ],
  "id": 1
}
```

Each result carries the `signature`, `slot`, `blockTime`, and `err`. Take a `signature` from the response for the next step.

## 2. Fetch one transaction

`getTransaction` returns the full, decoded transaction for a single signature.

{% tabs %}
{% tab title="curl" %}
```bash
curl https://<your-endpoint>.mainnet.rpcpool.com/<your-token> -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getTransaction",
    "params": ["SIGNATURE_FROM_STEP_1", { "encoding": "jsonParsed", "maxSupportedTransactionVersion": 0 }]
  }'
```
{% endtab %}

{% tab title="TypeScript" %}
```typescript
const res = await fetch("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    jsonrpc: "2.0",
    id: 1,
    method: "getTransaction",
    params: ["SIGNATURE_FROM_STEP_1", { encoding: "jsonParsed", maxSupportedTransactionVersion: 0 }],
  }),
});
console.log((await res.json()).result);
```
{% endtab %}
{% endtabs %}

### Expected response

The full transaction, decoded. Trimmed:

```json
{
  "jsonrpc": "2.0",
  "result": {
    "slot": 250000123,
    "blockTime": 1700000000,
    "meta": { "err": null, "fee": 5000, "preBalances": [ ... ], "postBalances": [ ... ] },
    "transaction": { "message": { ... }, "signatures": [ "5wHu1qwD4kLpXnR7e8oP..." ] }
  },
  "id": 1
}
```

This is the standard two-step pattern: one call to discover signatures, then one `getTransaction` per signature. For a busy address, that becomes an N+1 round-trip flow.

## 3. Get the whole history in one call

`getTransactionsForAddress` is Superbank's extension that collapses Steps 1 and 2 into a single request. Ask for full transactions and it returns them directly, with no per-signature follow-up.

{% tabs %}
{% tab title="curl" %}
```bash
curl https://<your-endpoint>.mainnet.rpcpool.com/<your-token> -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getTransactionsForAddress",
    "params": ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v", { "transactionDetails": "full", "sortOrder": "desc", "limit": 5 }]
  }'
```
{% endtab %}

{% tab title="TypeScript" %}
```typescript
const res = await fetch("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    jsonrpc: "2.0",
    id: 1,
    method: "getTransactionsForAddress",
    params: ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v", { transactionDetails: "full", sortOrder: "desc", limit: 5 }],
  }),
});
console.log((await res.json()).result);
```
{% endtab %}
{% endtabs %}

### Expected response

A `data` array of transactions plus a `paginationToken`, all in one response:

```json
{
  "jsonrpc": "2.0",
  "result": {
    "data": [
      {
        "slot": 250000001,
        "transactionIndex": 12,
        "blockTime": 1700000000,
        "transaction": { ... },
        "meta": { ... }
      }
    ],
    "paginationToken": "XHLKq2uNiGAJs3YhjnmnwT7LuJoTkGDqf1k4WJsnUd4A6e9bfB26vHr4dKLfMqwZuPmuXDHih4RaojCSt61os33"
  },
  "id": 1
}
```

Filters apply server-side and pagination uses a single cursor. For example, only successful transactions within a slot range:

```bash
curl https://<your-endpoint>.mainnet.rpcpool.com/<your-token> -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getTransactionsForAddress",
    "params": ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v", { "filters": { "status": "succeeded", "slot": { "gte": 250000000, "lt": 251000000 } } }]
  }'
```

Pass the `paginationToken` from the response in the next request to keep scanning; treat it as an opaque token. See [Historical data](https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/historical-data) for every option, filter, and response shape.

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-clock-rotate-left">:clock-rotate-left:</i> <strong>Historical data</strong></td><td>Purpose-built historical backend: millisecond reads from genesis across the full ledger, plus gRPC replay of every block.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/historical-data">Historical data</a></td></tr><tr><td><i class="fa-list-check">:list-check:</i> <strong>Best practices</strong></td><td>Query history efficiently: pagination, filters, and cost control at scale.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/historical-data/best-practices">Best practices</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
