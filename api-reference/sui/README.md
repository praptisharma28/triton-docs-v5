---
description: >-
  Full-service Sui API access on Triton: JSON-RPC, gRPC, an archival service,
  Walrus, and Seal.
---

# Overview

Triton has run a Sui validator since genesis and provides full-service Sui API access on mainnet and testnet.

## Interfaces

| Interface     | Status                     | Notes                                                                                                                                                                |
| ------------- | -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| JSON-RPC      | Live (deprecating in 2026) | The standard Sui JSON-RPC, documented here. Mysten Labs is migrating to gRPC and GraphQL.                                                                            |
| gRPC          | Beta                       | The `LedgerService` gRPC interface; will replace JSON-RPC.                                                                                                           |
| Archival gRPC | Live                       | Long-term historical access via `LedgerService` at `archive.mainnet.sui.rpcpool.com:443`. Query it explicitly; the standard endpoint does not fall back to archival. |
| Walrus        | Live                       | Storage, publisher, and aggregator.                                                                                                                                  |
| Seal          | Live                       | Permissioned server (mainnet); open and permissioned (testnet).                                                                                                      |

The Sui WebSocket subscription methods (`suix_subscribeEvent`, `suix_subscribeTransaction`) are deprecated by Mysten Labs. Use them only if you already depend on them.

## Endpoint and auth

```
https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>
```

Authenticate with the `X-Token` header, or the token in the URL path, the same way as Solana. The archival service requires the `X-Token` header.

## Method groups

* **Read API**: objects, transactions, events, checkpoints, protocol state.
* **Write API**: execute, dry-run, and developer-inspect transaction blocks.
* **Coin query API**: balances, metadata, and supply.
* **Governance read API**: staking, validators, committee, reference gas price.
* **Extended API**: owned objects, dynamic fields, event/transaction queries, name service, subscriptions.
* **Move utils**: normalized Move modules, functions, and structs.
* **Transaction builder API**: the `unsafe_*` server-side transaction builders.
