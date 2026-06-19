---
description: >-
  The Sui gRPC API, with endpoints, authentication, available services, and
  grpcurl examples.
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

# gRPC

### Overview

The Sui gRPC API is a fast, type-safe, and efficient interface for interacting with the Sui blockchain. It replaces the deprecated JSON-RPC protocol and is now generally available. gRPC uses [Protocol Buffers](https://protobuf.dev/overview/) for compact, high-performance data serialisation, and supports both standard request-response calls and real-time server-side streaming -- no polling required.

> **Note:** JSON-RPC is deprecated and will be deactivated in July 2026. Migrating to gRPC is strongly recommended for all production integrations.

### When to use gRPC

gRPC is the best choice when you need:

* **High-performance, low-latency** point lookups for transactions, objects, and checkpoints
* **Real-time streaming** subscriptions (replaces deprecated WebSocket support)
* **Strongly typed clients** generated from `.proto` definitions in TypeScript, Go, Rust, and other languages
* **Scalable backend systems** -- indexers, exchanges, bots, and dashboards

For frontends, developer tooling, or flexible filtered historical queries, consider GraphQL RPC as an alternative.

> **Important:** gRPC full nodes do not automatically fall back to Archival Storage for historical data. If your integration requires deep historical lookups, use the Archival Storage and Services endpoint directly.

***

### Endpoints

#### Free test endpoint (mainnet, rate-limited)

We provide a free shared gRPC endpoint for testing and evaluation purposes. It is strictly rate-limited and intended for development use only, **not for production traffic**.

```
mainnet.sui.rpcpool.com:443
```

No authentication token is required for this endpoint.

***

#### Shared clients

Shared plan clients access gRPC via the standard shared endpoint. Your unique endpoint hostname is available in the [client panel](https://customers.triton.one/).

**Endpoint format:**

```
XXX.sui.rpcpool.com:443
```

Replace `XXX` with your specific endpoint slug shown in the panel.

**Authentication** is required via the `X-Token` header:

```
X-Token: <your-token>
```

Both your endpoint and token can be found in your [client panel](https://customers.triton.one/).

***

#### Dedicated clients

Dedicated plan clients have a private endpoint provisioned exclusively for their use.

**Endpoint format:**

```
XXX.sui.rpcpool.com:443
```

Replace `XXX` with your dedicated endpoint slug shown in the panel.

**Authentication** is required via the `X-Token` header:

```
X-Token: <your-token>
```

Both your endpoint and token can be found in your [client panel](https://customers.triton.one/).

***

### Authentication

All non-free endpoints require the `X-Token` header to be included with every request. You can find your token in the client panel.

When using `grpcurl`:

```bash
grpcurl \
  -H "X-Token: <your-token>" \
  XXX.sui.rpcpool.com:443 \
  list
```

When using a generated gRPC client (e.g. in Go or TypeScript), add the token as a metadata field on every call:

```
key:   "x-token"
value: "<your-token>"
```

***

### Available services

The Sui gRPC API is defined via `.proto` files available in the [`sui-apis` repository on GitHub](https://github.com/MystenLabs/sui-apis/tree/main/proto). The following services are available:

| Service                        | Purpose                                                                               |
| ------------------------------ | ------------------------------------------------------------------------------------- |
| `TransactionExecutionService`  | Submit and execute signed transactions on the Sui network                             |
| `LedgerService`                | Look up checkpoints, transactions, and objects from current state and recent history  |
| `StateService`                 | Query live onchain data: balances, owned objects, dynamic fields; dry-run simulations |
| `SubscriptionService`          | Stream live checkpoint updates in real time                                           |
| `MovePackageService`           | Access deployed Move package metadata and content                                     |
| `SignatureVerificationService` | Validate signatures (including zkLogin) outside of transaction execution              |
| `NameService`                  | Resolve SuiNS names to addresses and perform reverse lookups                          |

Use these `.proto` definitions to generate type-safe client libraries in your language of choice.

***

### Quickstart: grpcurl examples

[`grpcurl`](https://github.com/fullstorydev/grpcurl) is the easiest way to explore the API from the command line.

#### List available services

```bash
grpcurl \
  -H "X-Token: <your-token>" \
  XXX.sui.rpcpool.com:443 \
  list
```

#### List methods in LedgerService

```bash
grpcurl \
  -H "X-Token: <your-token>" \
  XXX.sui.rpcpool.com:443 \
  list sui.rpc.v2.LedgerService
```

#### Get the latest checkpoint

```bash
grpcurl \
  -H "X-Token: <your-token>" \
  -d '{}' \
  XXX.sui.rpcpool.com:443 \
  sui.rpc.v2.LedgerService/GetLatestCheckpoint
```

#### Get a transaction (events and effects)

```bash
grpcurl \
  -H "X-Token: <your-token>" \
  -d '{
    "digest": "<transaction-digest>",
    "read_mask": {
      "paths": ["effects", "events"]
    }
  }' \
  XXX.sui.rpcpool.com:443 \
  sui.rpc.v2.LedgerService/GetTransaction
```

#### Subscribe to live checkpoints (streaming)

```bash
grpcurl \
  -H "X-Token: <your-token>" \
  -d '{}' \
  XXX.sui.rpcpool.com:443 \
  sui.rpc.v2.SubscriptionService/SubscribeCheckpoints
```

***

### Key concepts

#### Field masks

Use `read_mask` to request only the fields you need, reducing response size and latency:

```json
{
  "read_mask": {
    "paths": ["digest", "effects", "events"]
  }
}
```

Pass `"*"` to request all fields. If `read_mask` is omitted, it defaults to `*`. In batch requests, only the top-level `read_mask` is respected.

#### Pagination

List APIs (owned objects, balances, etc.) return paginated results. Use `page_size` to control batch size and pass the returned `next_page_token` into the next request to continue. An empty `next_page_token` means you have reached the end. Keep all other request parameters constant between pages.

#### Encodings

* `Address` and `ObjectId`: 64-character hex with a `0x` prefix
* `Digest`: Base58 encoded
* `TypeTag` / `StructTag`: Canonical string format

#### Errors

Errors follow the [gRPC richer error model](https://grpc.io/docs/guides/error/#richer-error-model). Detailed error information is provided in the `grpc-status-details-bin` header as a Base64-encoded `google.rpc.Status` message.

***

### Resources

* [Sui gRPC Reference (official)](https://docs.sui.io/references/fullnode-protocol)
* [Proto definitions -- sui-apis on GitHub](https://github.com/MystenLabs/sui-apis/tree/main/proto)
* [Official: What is gRPC?](https://docs.sui.io/develop/accessing-data/grpc/what-is-grpc)
* [Official: Querying Data with gRPC](https://docs.sui.io/develop/accessing-data/grpc/using-grpc)
* [grpcurl tool](https://github.com/fullstorydev/grpcurl)

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
