---
description: >-
  Superbank, the RPC 2.0 historical module that serves the full Solana ledger as
  spec-compliant JSON-RPC from a ClickHouse backend.
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: false
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# Superbank

Superbank is Triton's historical-data backend. It ingests the complete Solana ledger into ClickHouse and serves it as spec-compliant Solana JSON-RPC. It is the historical module of RPC 2.0 and the successor to Old Faithful, and it is open source under AGPL at [github.com/solana-rpc/superbank](https://github.com/solana-rpc/superbank).

Because Superbank answers the standard Solana JSON-RPC methods, existing clients work unchanged. On a Triton endpoint where it is enabled, the historical methods you already call are served from Superbank automatically, with nothing to route or configure on your side.

{% hint style="info" %}
Superbank serves historical reads. For real-time data, use Dragon's Mouth (gRPC), Whirligig (WebSocket), Deshred (pre-execution), or Fumarole (persistent streams).
{% endhint %}

## What Superbank serves

The complete ledger from genesis: every block, transaction, and entry. Historical methods currently served from Superbank:

* `getTransaction`
* `getSignaturesForAddress`
* `getSignatureStatuses`
* `getTransactionsForAddress` (a Superbank extension, documented below)

The remaining historical methods are being migrated over from Old Faithful. Responses are spec-compliant, so no client changes are needed when a method moves to Superbank.

## How it works

Superbank runs as three independent layers, ingest, storage, and query. Each scales on its own, and the data can be partitioned and sharded across servers when a single node is not enough.

### Ingest

The ingestor decodes blocks and transactions into rows and writes them to ClickHouse with no filtering or transformation, so writes stay at maximum throughput. It is source-agnostic, with built-in support for Dragon's Mouth gRPC, BigTable, JSON-RPC, and Anza's Jetstreamer, so you can backfill from an existing archive and then switch to a live stream without rewriting the pipeline. Triton backfills the ledger with Jetstreamer, then stays at chain tip with a Dragon's Mouth gRPC stream at `confirmed` commitment.

### Storage

Reading history is an analytics workload: scan many rows, return a few columns, reach deep into the ledger, and let the server sort and filter. Superbank stores the data in ClickHouse, a columnar database, and leans on that for performance:

* **Columnar storage**, so a query reads only the columns it asks for and skips the rest.
* **Insert-time materialised views.** Each write to the `transactions` table also produces derived, pre-sorted copies, sorted the way each method reaches the data: by signature for `getSignatureStatuses`, by address for `getSignaturesForAddress`, and by token owner for `getTransactionsForAddress`.
* **Per-column compression** (Delta + ZSTD for sequential values like slot, ZSTD for large repetitive blobs).
* **Tiered storage** as data ages, from NVMe for recent epochs to HDD for older epochs to S3 for deep history, with no schema change.
* A **bloom filter on signature** lets a lookup skip chunks that cannot contain a given signature, and an optional `gsfa_hot` partitioning spreads reads for very high-traffic addresses such as the USDC mint.

Base tables `blocks_metadata` and `transactions` are keyed by slot, and views are read newest-first. The full table schemas are at [github.com/solana-rpc/superbank/tree/main/ddl](https://github.com/solana-rpc/superbank/tree/main/ddl).

### Query

The query layer is a standard Solana JSON-RPC server plus the `getTransactionsForAddress` extension. Each request is translated into SQL tuned to the tables' physical sort order. A **head cache** subscribes to an upstream gRPC stream and holds the newest slots in memory, returning the latest data in under 1 ms and making `processed` commitment available for historical methods. Repeat reads come from a query cache (`getTransaction` results carry a longer TTL because finalised transactions never change), and `getSlot` is answered from an in-memory slot synchroniser. Every response carries headers showing which backend served it (ClickHouse, the head cache, or both) and how much work it did (rows read, rows returned, bytes processed).

## getTransactionsForAddress

`getTransactionsForAddress` is designed for workloads that would otherwise call `getSignaturesForAddress` followed by `getTransaction` for every returned signature.

That two-step pattern works, but it creates an N+1 request flow: one request to discover signatures, then one more per transaction to fetch details. It also pushes filtering, pagination, token-account expansion, and result assembly onto the client.

This method combines those steps into a single address-history query. It can return either signature-level results or full transaction payloads, apply filters server-side, preserve a single pagination cursor, and optionally include token-account activity owned by the requested address.

It is a custom RPC method, not part of the standard Solana JSON-RPC API, but it follows the same JSON-RPC request and response envelope.

### Request

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getTransactionsForAddress",
  "params": [
    "AddressBase58",
    {
      "transactionDetails": "signatures",
      "sortOrder": "desc",
      "limit": 100,
      "paginationToken": null,
      "filters": {
        "slot": { "gte": 250000000 },
        "status": "any",
        "tokenAccounts": "none"
      }
    }
  ]
}
```

### Parameters

| Parameter | Type   | Required | Description                              |
| --------- | ------ | -------- | ---------------------------------------- |
| address   | string | Yes      | Base58 Solana account address to search. |
| options   | object | No       | Query options and filters.               |

### Options

| Option                         | Type                                   | Default                           | Description                                  |
| ------------------------------ | -------------------------------------- | --------------------------------- | -------------------------------------------- |
| transactionDetails             | signatures \| full                     | signatures                        | Controls response detail.                    |
| sortOrder                      | asc \| desc                            | desc                              | Sort by slot and transaction position.       |
| limit                          | number                                 | 1000 for signatures, 100 for full | Maximum number of results.                   |
| paginationToken                | string                                 | null                              | Token returned from a previous response.     |
| commitment                     | confirmed \| finalized                 | finalized                         | Commitment level.                            |
| minContextSlot                 | number                                 | none                              | Fails if the node has not reached this slot. |
| encoding                       | json \| jsonParsed \| base58 \| base64 | json                              | Used when transactionDetails is full.        |
| maxSupportedTransactionVersion | number                                 | none                              | Used when transactionDetails is full.        |
| filters                        | object                                 | none                              | Optional filters.                            |

### Filters

| Filter        | Type                          | Description                                                          |
| ------------- | ----------------------------- | -------------------------------------------------------------------- |
| slot          | comparison object             | Filter by slot. Supports gte, gt, lte, lt.                           |
| blockTime     | comparison object             | Filter by block time. Supports gte, gt, lte, lt, eq.                 |
| signature     | comparison object             | Filter by transaction signature position. Supports gte, gt, lte, lt. |
| status        | any \| succeeded \| failed    | Filter by transaction status.                                        |
| tokenAccounts | none \| balanceChanged \| all | Include token-owner activity for the address.                        |

Comparison object example:

```json
{
  "gte": 250000000,
  "lt": 260000000
}
```

### Token account filtering

`tokenAccounts` controls whether token-account activity owned by the requested address is included.

| Value          | Behaviour                                                                 |
| -------------- | ------------------------------------------------------------------------- |
| none           | Only transactions where the address appears directly.                     |
| all            | Also include transactions involving token accounts owned by the address.  |
| balanceChanged | Include owned token-account transactions only when token balance changed. |

Token-owner activity is derived from transaction pre/post token balance metadata.

### Response

{% tabs %}
{% tab title="Signatures" %}
When `transactionDetails` is `signatures`, each result contains transaction metadata.

```json
{
  "jsonrpc": "2.0",
  "result": {
    "data": [
      {
        "signature": "TransactionSignatureBase58",
        "slot": 250000001,
        "transactionIndex": 12,
        "err": null,
        "memo": null,
        "blockTime": 1700000000,
        "confirmationStatus": "finalized"
      }
    ],
    "paginationToken": "250000001:12"
  },
  "id": 1
}
```
{% endtab %}

{% tab title="Full transactions" %}
When `transactionDetails` is `full`, each result contains the encoded transaction and metadata.

```json
{
  "jsonrpc": "2.0",
  "result": {
    "data": [
      {
        "slot": 250000001,
        "transactionIndex": 12,
        "blockTime": 1700000000,
        "transaction": {},
        "meta": {}
      }
    ],
    "paginationToken": "250000001:12"
  },
  "id": 1
}
```
{% endtab %}
{% endtabs %}

### Pagination

Use the returned `paginationToken` in the next request to continue scanning.

```json
{
  "jsonrpc": "2.0",
  "id": 2,
  "method": "getTransactionsForAddress",
  "params": [
    "AddressBase58",
    {
      "limit": 100,
      "paginationToken": "250000001:12"
    }
  ]
}
```

### Example: successful transactions only

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getTransactionsForAddress",
  "params": ["AddressBase58", { "filters": { "status": "succeeded" } }]
}
```

### Example: token balance changes

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getTransactionsForAddress",
  "params": ["OwnerAddressBase58", { "filters": { "tokenAccounts": "balanceChanged" } }]
}
```

### Example: slot range

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getTransactionsForAddress",
  "params": ["AddressBase58", { "filters": { "slot": { "gte": 250000000, "lt": 251000000 } } }]
}
```

## Self-hosting

Superbank is open source under AGPL, so you can run, audit, and extend it yourself. Its source-agnostic ingest means you can backfill from BigTable or an existing archive and then switch to a live stream.

* Source and schemas: [github.com/solana-rpc/superbank](https://github.com/solana-rpc/superbank)
* Walkthrough: [Index Solana history with Superbank](https://kate-6.gitbook.io/triton-one-docs-v5/guides/solana/how-tos/index-solana-history-with-superbank)

Prefer not to operate it? Triton runs Superbank as a managed, globally distributed service. [Get an endpoint](https://customers.triton.one/onboarding).

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
