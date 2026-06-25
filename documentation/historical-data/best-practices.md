---
description: 'Query historical Solana data efficiently: address history, server-side filtering, pagination, and large backfills.'
---

# Best practices

How to query historical Solana data efficiently: address history, filtering, pagination, and large backfills.

## Pull address history in one call

* **Use `getTransactionsForAddress` instead of the `getSignaturesForAddress` + `getTransaction` two-step.** The standard pattern is N+1 (one call to list signatures, then one per signature); `getTransactionsForAddress` collapses it into a single request.
* **Set `tokenAccounts` to `all` or `balanceChanged` to capture token activity.** The default `none` returns only transactions where the address appears directly, not transactions involving token accounts it owns.

## Filter and paginate server-side

* **Pass server-side filters (`slot`, `blockTime`, `signature`, `status`) so you receive only the slice you need.** Filtering at the source instead of in your client saves bandwidth and client-side compute.
* **Bound deep scans with a slot range (`slot: { gte, lt }`) and page with the returned `paginationToken`.** Feed each response's `paginationToken` into the next request to continue scanning.
* **Use `minContextSlot` when you need recent data fast.** The newest slots are served from an in-memory head cache in under 1 ms, so recent lookups do not hit the archive.

## Index large history with streaming, not polling

* **To backfill or index large stretches of history, stream with Jetstreamer instead of polling Superbank.** Jetstreamer is open-source and performant, and supports filtering, custom storage backends, and existing Geyser plugins.
* **For server-side-filtered bulk pulls, use Old Faithful's `StreamTransactions` / `StreamBlocks`** so you receive only the relevant slice instead of fetching everything.
* **Self-host the archive if you have the ops capacity.** Store long-term data as CAR (Content Addressable aRchive) files on any S3-compatible platform, and verify archive integrity before trusting it: Triton validated the archive from genesis and open-sourced the verification tooling.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
