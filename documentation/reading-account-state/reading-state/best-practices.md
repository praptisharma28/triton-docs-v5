# Best practices

How to read Solana account, program, and token state quickly and reliably. For real-time changes see real-time streaming; for data older than the last few epochs see historical data.

## Batch and trim your reads

* **Read many accounts in one `getMultipleAccounts` call** instead of many `getAccountInfo` calls. Results come back in input order, with `null` for any address not found.
* **Request only the bytes you need with `dataSlice`** (`{ offset, length }`) rather than the whole account.
* **Set `encoding` to `base64`.** The default binary and `base58` only work for account data under 128 bytes; larger accounts return `-32602` telling you to switch.
* Address-keyed lookups (`getAccountInfo`, `getMultipleAccounts`, `getBalance`, `getTokenAccountBalance`) resolve directly by address with no filter index, so they stay fast.

## Route heavy queries to an index, not a full scan

* **Don't run unfiltered `getProgramAccounts` against custom programs on the shared service.** Shared Agave nodes only keep secondary indexes for SPL Token and Token-2022; any other program must scan the entire account set, which the shared pool rejects (`excluded from account secondary indexes; this RPC method unavailable for key`).
* **Serve heavy `getProgramAccounts` through Cloudbreak indexed reads** (dedicated nodes). The Postgres-backed index is tuned per program and `memcmp` offset and typically drops query times from seconds to milliseconds, even for programs with millions of accounts.
* **`getLargestAccounts` is disabled on the shared service** (returns HTTP `410`); it scans the whole account set on every call. Request a dedicated node if you need it.

## Read token data the supported way

* **Use the token methods** (`getTokenAccountsByOwner`, `getTokenAccountsByDelegate`, `getTokenAccountsByMint`), not a raw `getProgramAccounts` over the Token program. They hit purpose-built indexes; an unfiltered scan of the whole Token program is not a supported access pattern.
* **If you must use `getProgramAccounts` on the original Token program, include `dataSize: 165`** to select only valid token accounts. For Token-2022 (variable size), omit `dataSize` and filter on the byte at offset 165 instead.
* **Submit `getProgramAccounts`, `getTokenAccountsByOwner`, and `getTokenAccountsByDelegate` as individual requests.** They are always unbatched, so batch ordering and atomicity do not apply.

## Use the Digital Assets API for assets

* **For NFTs, compressed NFTs, and rich token queries, use DAS.** Decompression and caching happen server-side, across SPL Token and Token-2022 in one interface.
* **`getAssetsByOwner` is the more efficient replacement for `getTokenAccountsByOwner`** when listing what an address holds.
* **Batch and paginate:** `getAssets` / `getAssetProofs` fetch many in one request; `getTokenAccounts` lists all holders of a mint; page with `page`/`limit` (or `cursor`/`before`/`after`).
* **`getAssetsByGroup` is temporarily disabled** for performance work; do not call it until it is re-enabled.

## Get freshness right

* **A read is only final at `confirmed` or `finalized`.** `processed` slots can be dropped during minority forks or congestion, and indexed reads reject `processed` by default (`-32003`).
* **Use `minContextSlot` to fail fast on stale data.** Pass a slot you have already seen and a later call errors with `-32000` if the index has not reached it, rather than returning older state.
* **Use `withContext: true` on `getProgramAccounts`** when you need the slot a result corresponds to. The account, token, and balance methods already include it.
* **Repeated identical queries are cached.** On Cloudbreak, each query shape keeps its serialized response and only re-reads changed accounts, so frequent polling of the same query stays cheap. Reuse consistent `memcmp`/`dataSize` filter shapes so an index keeps matching.

## Stream instead of polling for live data

* **For account or transaction changes as they happen, use Dragon's Mouth (gRPC), not polling.** Point-in-time reads answer "now"; a stream pushes updates as they occur.
* **Or skip the rewrite with Account Sync.** The Triton SDK is a drop-in `@solana/web3.js` replacement that keeps account state live in a local buffer fed by a stream, so `getAccountInfo` stays current without polling and without changing your read code. See Account Sync.
* **For program-specific data, use Program Data Streams (Vixen)** rather than repeatedly fetching and parsing. Polling is slower, resource-heavy, and prone to missing fast-moving events.
* **In a browser, use Whirligig WebSockets.** gRPC is server-to-server only and is not supported in web browsers.

## Manage client connections

* **Reuse a pool of persistent connections; don't open a new one per request.** NodeJS `fetch`/`undici` opens a fresh socket per request and exhausts ephemeral ports under concurrent load, causing `fetch failed` / `ECONNRESET` / `Connect Timeout`.
* **Start with a 50-connection pool** (a global undici dispatcher or `https.Agent` with `keepAlive`), then raise only if all 50 are consistently in use. Pooling also lowers load on the RPC servers.

## Stay within rate limits

* **On HTTP `429`, pause all requests from that IP for the full 10-second window before retrying.** Retrying sooner only returns more 429s and never clears the limit.
* **Budget expensive methods separately.** The shared default is 1,200 requests per 10 seconds per IP, but computationally expensive methods like `getProgramAccounts` have significantly lower limits.
* **Monitor `X-Ratelimit-*` response headers** and check `https://<your-endpoint>.rpcpool.com/ratelimits` for your endpoint's exact numbers.
* **Authenticate backends with a token** (a more favourable rate limit, since one IP serves many users) and **use allowed origins for browsers** (never embed a secret token in frontend code). Run backend scripts and bots on a dedicated node; the shared service is for frontend dApp traffic only.

## Avoid removed and excluded methods

* **Audit out the methods removed in Solana v2.0** and use their successors (for example `getStakeActivation` → `getAccountInfo`; confirmed-block / confirmed-transaction calls → `getBlock` / `getTransaction`). Removed methods no longer function.
* **Don't read Light Protocol / ZK Compression accounts via `getProgramAccounts`** (they are excluded); consume them through the Photon indexer instead.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
