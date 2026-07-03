---
description: Read Solana's complete history faster and easier with Superbank.
layout:
  pagination:
    visible: true
---

# Quickstart

This guide will walk you through the most common history read, `getTransaction`, and Triton's extension `getTransactionsForAddress`. All historical methods are routed through the Superbank backend automatically, and responses are 100% spec-compliant with Solana JSON-RPC, so it requires zero changes to your existing code.

## 0. Prerequisites

* An active Triton subscription
* Your endpoint URL and secret token from the [customer dashboard](https://customers.triton.one)
* `curl` or any Solana JSON-RPC client

## 1. Fetch a transaction

`getTransaction` returns the full, decoded transaction for a single signature. Take a signature from your app or an explorer.

{% tabs %}
{% tab title="curl" %}
```bash
curl https://<your-endpoint>.mainnet.rpcpool.com/<your-token> -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getTransaction",
    "params": ["YOUR_SIGNATURE", { "encoding": "jsonParsed", "maxSupportedTransactionVersion": 0 }]
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
    params: ["YOUR_SIGNATURE", { encoding: "jsonParsed", maxSupportedTransactionVersion: 0 }],
  }),
});
console.log((await res.json()).result);
```
{% endtab %}

{% tab title="Response" %}
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
{% endtab %}
{% endtabs %}

If you already know the transaction's slot, pass Triton's optional `slot` hint. Superbank then queries that exact slot instead of searching the whole ledger for the signature, cutting response time by roughly 50%:

```bash
curl https://<your-endpoint>.mainnet.rpcpool.com/<your-token> -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getTransaction",
    "params": ["YOUR_SIGNATURE", { "encoding": "jsonParsed", "maxSupportedTransactionVersion": 0, "slot": 250000123 }]
  }'
```

The response is `null` if the signature isn't present in that slot. `getTransactionsForAddress` below returns each item's `slot`, so its output feeds the hint directly.

## 2. Get the whole history in one call

`getTransactionsForAddress` is Superbank's extension that collapses the usual list-then-fetch pattern (`getSignaturesForAddress`, then `getTransaction` per signature) into a single request. Ask for full transactions and it returns them directly, with no per-signature follow-up.

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

{% tab title="Response" %}
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
{% endtab %}
{% endtabs %}

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
