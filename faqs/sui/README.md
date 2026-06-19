---
description: >-
  Common questions about running on Sui: endpoints, gRPC, archival history,
  Walrus, and Seal.
---

# General

## Networks and access

<details>

<summary>Which Sui networks does Triton support?</summary>

Triton runs on Sui Mainnet and Testnet. Mainnet offers gRPC, JSON-RPC (being deprecated), Walrus storage, and a permissioned Seal server. Testnet adds an open Seal server alongside the permissioned one.

</details>

<details>

<summary>How do I connect to a Sui gRPC endpoint?</summary>

Point your gRPC client at `<your-endpoint>.sui.rpcpool.com:443` (your endpoint slug comes from the [customer dashboard](https://customers.triton.one)) and send your token on every call in the `X-Token` header:

```sh
grpcurl -H "X-Token: <your-token>" <your-endpoint>.sui.rpcpool.com:443 list
```

</details>

<details>

<summary>Is there a free Sui endpoint for testing?</summary>

Yes. `mainnet.sui.rpcpool.com:443` is a free, shared, rate-limited gRPC endpoint that needs no token. It's for development and evaluation only, not production traffic.

</details>

## gRPC, JSON-RPC, and streaming

<details>

<summary>Should I use gRPC or JSON-RPC?</summary>

Use gRPC. It's generally available and supports both unary point lookups and real-time server-side streaming. JSON-RPC still works but is deprecated and scheduled for deactivation on 31 July 2026, so migrate production integrations to gRPC.

</details>

<details>

<summary>What gRPC services are available on Sui?</summary>

The standard Sui gRPC services: LedgerService (checkpoints, transactions, objects), StateService (balances, owned objects, dynamic fields, dry-run), TransactionExecutionService, SubscriptionService (live checkpoint streaming), MovePackageService, SignatureVerificationService (including zkLogin), and NameService (SuiNS).

</details>

<details>

<summary>Can I stream live data on Sui?</summary>

Yes. The gRPC SubscriptionService streams checkpoints in real time. This replaces the legacy WebSocket and PubSub API that Mysten Labs is retiring.

</details>

## Archival history

<details>

<summary>How do I access full Sui history?</summary>

Sui full nodes prune older data. The archival service serves the complete history from genesis (transactions, checkpoints, object states, effects, and events) over the same gRPC LedgerService interface. Query it explicitly: a common pattern is to hit your full node first and retry on archival when it returns NotFound.

</details>

<details>

<summary>What are the archival endpoints?</summary>

Shared plan: `archive.mainnet.sui.rpcpool.com:443`. Dedicated plan: `archive-<your-slug>.sui.rpcpool.com:443`. Both use the same `X-Token` header as your standard endpoint.

</details>

## Walrus and Seal

<details>

<summary>What is Walrus, and what does Triton run?</summary>

Walrus is a decentralised, fault-tolerant storage layer for large binary blobs that uses Sui for coordination. Triton hosts the full stack: publishers (write), aggregators (read), and storage nodes.

</details>

<details>

<summary>What is Seal?</summary>

Seal is a decentralised secret-management and threshold-encryption service that works with Walrus, with access control enforced on Sui. Triton runs an open Seal server on Testnet for experiments, and permissioned Seal servers (controlled access, higher throughput) for production.

</details>

## Getting started

<details>

<summary>How do I get started on Sui?</summary>

Sign up at [customers.triton.one](https://customers.triton.one), get your endpoint slug and token from the dashboard, then use gRPC (recommended) via `grpcurl` or a generated client against `<your-endpoint>.sui.rpcpool.com:443` with the `X-Token` header. For history beyond node retention, also target the archival endpoint. One account covers Solana, Sui, and Monad.

</details>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
