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
* **`getSignaturesForAddress` paginates with `before`/`until` signature cursors**, plus Triton's `beforeSlot`/`untilSlot` whole-slot bounds when you already know the slot range. It has no content filters.
* **`getTransaction` has no filters or pagination.** If you already know the transaction's slot, pass Triton's optional `slot` hint to skip the signature-to-slot lookup and get the response faster.
* **`minContextSlot` is a consistency guard, not a speed lever.** It fails the request if the server has not reached that slot yet, protecting you from stale reads. Recent reads are fast on their own: the newest slots are served from the in-memory head cache in under 1 ms.

## Backfill large history with streaming, not polling

Polling `getBlock` and `getTransaction` over HTTP for months of history is the slow path: use it only for small slot ranges. For real backfills, stream:

* **Jetstreamer + the public Old Faithful archive, for the largest backfills.** [Jetstreamer](https://github.com/anza-xyz/jetstreamer) is Anza's open-source backfilling toolkit that streams the full ledger from [Old Faithful](https://old-faithful.net/), the open public-good archive of every Solana block and transaction from genesis to tip. It replays history highly parallelised (over 2.7M TPS with strong hardware) into Jetstreamer or Geyser plugins. One limitation to plan around: Old Faithful carries no account updates, so Jetstreamer does not either. The [self-hosting walkthrough](https://app.gitbook.com/s/TpqU5Dqc6tdzY8J23dd7/solana/how-tos/index-solana-history-with-superbank) uses this path to backfill Superbank.
* **Superbank's gRPC streams, for server-side-filtered pulls.** `StreamBlocks` and `StreamTransactions` replay bounded slot ranges straight from ClickHouse, with account include/exclude/required, vote, and success/failure filters, so you receive only the relevant slice instead of fetching everything.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
