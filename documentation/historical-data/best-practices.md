---
description: 'Query historical Solana data efficiently: address history, server-side filtering, pagination, and large backfills.'
---

# Best practices

## Pull address history in one call

* **Use `getTransactionsForAddress` instead of the `getSignaturesForAddress` + `getTransaction` two-step.** The standard pattern is N+1 (one call to list signatures, then one per signature); `getTransactionsForAddress` collapses it into a single request.
* **Set `tokenAccounts` to `all` or `balanceChanged` to capture token activity.** The default `none` returns only transactions where the address appears directly, not transactions involving token accounts it owns.

## Filter and paginate server-side

Server-side filtering is per-method, so use what each method actually supports:

* **`getTransactionsForAddress` carries the full filter set.** Pass the `slot`, `blockTime`, `signature`, `status`, and `tokenAccounts` filters so you receive only the slice you need, and page with the returned `paginationToken`. Bound deep scans with a slot range (`slot: { gte, lt }`).
* **`getSignaturesForAddress` paginates with `before`/`until` signature cursors**, plus Triton's `beforeSlot`/`untilSlot` whole-slot bounds when you already know the slot range: they skip the signature-to-slot lookup the standard cursors force on the server. It has no content filters.
* **`getTransaction` has no filters or pagination, so pass the `slot` hint when you know it.** A signature carries no timing information, so an unhinted lookup searches the entire ledger for it. Triton's optional `slot` parameter narrows the search to that one slot, cutting response time by roughly 50% and sparing the server a fall-back to deeper, costlier storage.
* **`minContextSlot` is a consistency guard, not a speed lever.** It fails the request if the server has not reached that slot yet, protecting you from stale reads. Recent reads are fast on their own: the newest slots are served from the in-memory head cache in under 1 ms.

## Backfill large history with streaming

Polling `getBlock` and `getTransaction` over HTTP for months of history is the slow path: use it only for small slot ranges. For heavy or full-chain backfills, we recommend two streaming paths:

* **Jetstreamer + the public Old Faithful archive.** [Jetstreamer](https://github.com/anza-xyz/jetstreamer), Anza's open-source backfilling toolkit, streams from [Old Faithful](https://old-faithful.net/), the open public-good archive of every Solana block and transaction. It replays history highly parallelised (over 2.7M TPS with strong hardware) into ClickHouse or any other sink you choose. See the [guide](https://app.gitbook.com/s/TpqU5Dqc6tdzY8J23dd7/solana/how-tos/index-solana-history-with-superbank) for more details.
* **Superbank's gRPC streams.** Managed `StreamBlocks` and `StreamTransactions` methods for replaying bounded slot ranges straight from ClickHouse with no operational overhead, plus account include/exclude/required, vote, and success/failure filters.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
