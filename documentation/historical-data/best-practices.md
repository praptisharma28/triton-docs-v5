---
description: Practical tips for querying old Solana transactions, blocks, and address history efficiently.
---

# Best practices

How to query historical and archival Solana data: old transactions, blocks, and address history.

## Let Triton route history for you

* **Use Triton's endpoints rather than running your own archive.** Every Solana endpoint can query the full history back to the genesis block.
* **Just make the normal call** (`getTransaction`, `getBlock`). Recent data (the last several epochs) is served from the low-latency backends; older data is automatically forwarded to the long-term archive (Superbank and Old Faithful). You do not route it yourself.
* **Expect deeper queries to be slower.** Only very old data traverses the archival ledger systems.
* **Budget for archive pricing on very old data:** Superbank is the final step for the oldest lookups, at $10.00 per million queries ($10.00/month minimum if used), across `getTransaction`, `getBlock`, `getSignaturesForAddress`, `getTransactionsForAddress`.

## Pull address history in one call

* **Prefer `getTransactionsForAddress` over the `getSignaturesForAddress` + `getTransaction` two-step.** The standard pattern is an N+1 flow (one call to find signatures, then one per signature); the single method collapses it.
* **Filter server-side**, not in the client: scope by slot, blockTime, signature, and status, and choose signature-level or full payloads in one query.
* **Bound deep scans with a slot range** (`slot: { gte, lt }`) and **page with the returned `paginationToken`**, feeding it into the next request.
* **To capture full token activity, set `tokenAccounts` to `all`** (or `balanceChanged`). The default `none` returns only transactions where the address appears directly.

## Use Old Faithful for bulk history

* **Use Old Faithful for bulk transaction and block data** from any past epoch back to genesis. It is the open-source archive and exposes both standard JSON-RPC and high-performance gRPC streams.
* **A historical `getBlock` / `getTransaction` on your subscription routes to Old Faithful automatically** when required.
* **Filter large datasets server-side with `StreamTransactions` / `StreamBlocks`** so you receive only the relevant slice instead of fetching everything.
* **Backfill large amounts with Jetstreamer**, the recommended tool: it supports filtering, customizable storage backends, and existing geyser plugins.

## Self-hosting and integrity

* **The hosted Old Faithful gRPC interface is retired** (an updated version is in progress). You can still self-host it; contact support@triton.one for help.
* **Choose managed endpoints unless you can provision significant storage** to run your own archive node.
* **For self-hosted long-term storage, use CAR (Content Addressable aRchive) files** on any S3-compatible platform.
* **Verify archive integrity before trusting historical data.** Triton validated the archive from genesis and open-sourced the tooling to verify it.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
