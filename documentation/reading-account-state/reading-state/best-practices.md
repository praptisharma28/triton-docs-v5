---
description: How to keep Solana account, program, and token reads fast and fresh at scale.
---

# Best practices

## Batch and trim your reads

* **Read many accounts in one `getMultipleAccounts` call** instead of many `getAccountInfo` calls. Results come back in input order, with `null` for any address not found.
* **Request only the bytes you need with `dataSlice`** (`{ offset, length }`) rather than the whole account.
* **Set `encoding` to `base64`.** The default binary and `base58` only work for account data under 128 bytes; larger accounts return `-32602` telling you to switch.
* **Filter `getProgramAccounts` with `memcmp` and `dataSize`.** On Triton, filtered queries are indexed automatically by Cloudbreak, with nothing to route or configure, so they return in milliseconds instead of scanning the program's full account set.


## Read token data the supported way

* **Use the token methods** (`getTokenAccountsByOwner`, `getTokenAccountsByDelegate`, `getTokenAccountsByMint`), not a raw `getProgramAccounts` over the Token program.
* **If you must use `getProgramAccounts` on the original Token program, include `dataSize: 165`** to select only valid token accounts. For Token-2022 (variable size), omit `dataSize` and filter on the byte at offset 165 instead.


## Get freshness right

* **A read is only final at `confirmed` or `finalized`.** `processed` slots can be dropped during minority forks or congestion, and indexed reads reject `processed` by default (`-32003`).
* **Use `minContextSlot` to fail fast on stale data.** Pass a slot you have already seen and a later call errors with `-32000` if the index has not reached it, rather than returning older state.
* **Use `withContext: true` on `getProgramAccounts`** when you need the slot a result corresponds to. The account, token, and balance methods already include it.

## Stream instead of polling for live data

* **For account or transaction changes as they happen, use Dragon's Mouth (Yellowstone gRPC), not polling.** Point-in-time reads answer "now"; a stream pushes updates as they occur.
* **In a browser, use Whirligig WebSockets.** gRPC is server-to-server only and is not supported in web browsers.
* **Or skip the rewrite with Account Sync.** The Triton SDK is a drop-in `@solana/web3.js` replacement that keeps account state live in a local buffer fed by a stream, so `getAccountInfo` stays current without polling and without changing your read code. See Account Sync.
* **If your workload can't miss data (indexers, analytics, accounting), use Fumarole.** It persists your stream position with 4 days of retention and an at-least-once delivery guarantee, so you can disconnect, resume where you left off, and backfill anything you missed.

## Manage client connections

* **Reuse a pool of persistent connections; don't open a new one per request.** NodeJS `fetch`/`undici` opens a fresh socket per request and exhausts ephemeral ports under concurrent load, causing `fetch failed` / `ECONNRESET` / `Connect Timeout`.
* **Start with a 50-connection pool** (a global undici dispatcher or `https.Agent` with `keepAlive`), then raise only if all 50 are consistently in use. Pooling also lowers load on the RPC servers.

## Stay within rate limits

* **On HTTP `429`, pause all requests from that IP for the full 10-second window before retrying.** Retrying sooner only returns more 429s and never clears the limit.
* **Budget expensive methods separately.** The shared default is 1,200 requests per 10 seconds per IP, but computationally expensive methods like `getProgramAccounts` have significantly lower limits.
* **Monitor `X-Ratelimit-*` response headers** and check `https://<your-endpoint>.rpcpool.com/ratelimits` for your endpoint's exact numbers.
* **Authenticate backends with a token** (a more favourable rate limit, since one IP serves many users) and **use allowed origins for browsers** (never embed a secret token in frontend code).

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
