---
description: Query Solana's complete transaction history on Triton with getSignaturesForAddress, getTransaction, and the getTransactionsForAddress extension.
layout:
  pagination:
    visible: false
---

# Quickstart

Read Solana's complete on-chain history on Triton. Historical methods are served from Superbank automatically, as spec-compliant Solana JSON-RPC, so the calls you already know work unchanged. This reads an address's history two ways: the standard `getSignaturesForAddress` then `getTransaction` pattern, and the `getTransactionsForAddress` extension that returns it in a single call.

## Step 0. Prerequisites

* A Triton Solana endpoint and token with historical data enabled, from your [customer dashboard](https://customers.triton.one).
* `curl`, or any Solana JSON-RPC client.

## Step 1. List an address's signatures

`getSignaturesForAddress` returns the transaction signatures that touch an address, newest first.

```bash
curl https://<your-endpoint>.mainnet.rpcpool.com/<your-token> -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getSignaturesForAddress",
    "params": ["AddressBase58", { "limit": 5 }]
  }'
```

Each result carries the `signature`, `slot`, `blockTime`, and `err`. Take a `signature` from the response for the next step.

## Step 2. Fetch one transaction

`getTransaction` returns the full, decoded transaction for a single signature.

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

This is the standard two-step pattern: one call to discover signatures, then one `getTransaction` per signature. For a busy address, that becomes an N+1 round-trip flow.

## Step 3. Get the whole history in one call

`getTransactionsForAddress` is Superbank's extension that collapses Steps 1 and 2 into a single request. Ask for full transactions and it returns them directly, with no per-signature follow-up.

```bash
curl https://<your-endpoint>.mainnet.rpcpool.com/<your-token> -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getTransactionsForAddress",
    "params": ["AddressBase58", { "transactionDetails": "full", "sortOrder": "desc", "limit": 5 }]
  }'
```

Filters apply server-side and pagination uses a single cursor. For example, only successful transactions within a slot range:

```bash
curl https://<your-endpoint>.mainnet.rpcpool.com/<your-token> -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getTransactionsForAddress",
    "params": ["AddressBase58", { "filters": { "status": "succeeded", "slot": { "gte": 250000000, "lt": 251000000 } } }]
  }'
```

Pass the `paginationToken` from the response in the next request to keep scanning. See [Historical data](https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/historical-data) for every option, filter, and response shape.

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-database">:database:</i> <strong>Historical data</strong></td><td>The full method list, how Superbank works, and the getTransactionsForAddress reference.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/historical-data">Historical data</a></td></tr><tr><td><i class="fa-list-check">:list-check:</i> <strong>Best practices</strong></td><td>Query history efficiently at scale.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/historical-data/best-practices">Best practices</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
