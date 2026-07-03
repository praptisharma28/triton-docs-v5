---
description: Flexible limits across all the networks, services, and infrastructure types.
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
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# Rate and connection limits

We apply limits per IP to keep legitimate application traffic flowing smoothly while preventing abuse:

* **Rate limits** cap how many HTTP RPC requests an IP can send in a 10-second window.
* **Connection limits** cap the number of simultaneous streaming connections an IP can open and the number of subscriptions each connection can hold.

Standard limits apply to every network Triton serves. Some Solana RPC methods have additional limits applied on top.

New accounts on the shared service default to a tier sized for typical workloads (frontends, indexers, light backends). If your traffic is heavier, we can raise yours on request at no extra cost.

## Rate limits (JSON-RPC)

Two request budgets apply to your endpoint at the same time, resetting every 10 seconds:

* **Total requests.** Send up to 1,200 requests across all methods (e.g. your IP can send 600 `getBalance` and 600 `getSlot` during a 10s window).
* **Per-method limits.** Heavy Solana methods have a lower cap applied on top (e.g. you can hit a `getProgramAccounts` cap after 50 requests, while your total is still under 1,200)

{% hint style="warning" %}
If either check fails, you get `HTTP 429 Too Many Requests`. Pause all requests from that IP for the full 10-second window before retrying. Retrying sooner only returns more 429s and won't clear the limit.
{% endhint %}

### Solana per-method limits

Methods not listed here fall back to the 1,200 total. Each method below has its own cap per 10 seconds:

| Method                        | Per 10s                               |
| ----------------------------- | ------------------------------------- |
| `getClusterNodes`             | 400                                   |
| `getMultipleAccounts`         | 400                                   |
| `getVoteAccounts`             | 400                                   |
| `getBlock`                    | 200                                   |
| `getConfirmedTransaction`     | 200                                   |
| `getLeaderSchedule`           | 200                                   |
| `getLatestBlockhash`          | 100                                   |
| `getRecentPerformanceSamples` | 100                                   |
| `getTokenAccountsByOwner`     | 100                                   |
| `sendTransaction`             | 100                                   |
| `getProgramAccounts`          | 50                                    |
| `getLargestAccounts`          | 0 (disabled on shared infrastructure) |
| `getTokenLargestAccounts`     | 0 (disabled on shared infrastructure) |

## Connection limits (Streaming)

Streaming caps the number of **simultaneous connections and subscriptions per IP**. A single connection can have many subscriptions.

| Pool                                    | Concurrent connections per IP | Subscriptions per IP |
| --------------------------------------- | :---------------------------: | :------------------: |
| Shared, WebSocket (Whirligig)           |              250              |        200,000       |
| Shared, gRPC (Dragon's Mouth, Fumarole) |              400              |        200,000       |
| Dedicated node                          |             no cap            |        no cap        |

Dedicated nodes run on isolated hardware and apply no connection or subscription cap: they carry as many connections as the node can serve.

## Query your limits in real-time

Every endpoint exposes its current limits at `/ratelimits`:

```bash
curl https://<your-endpoint>/<your-token>/ratelimits
```

The response is JSON, listing the request budget per window, every per-method override, and the connection cap. It's the source of truth for **your** endpoint's exact numbers, which can differ from the defaults above if our team has tuned them.

Every JSON-RPC response also carries `X-Ratelimit-*` headers. Watch them in your client to back off **before** you hit a 429:

| Header                         | What it tells you                             |
| ------------------------------ | --------------------------------------------- |
| `X-Ratelimit-Limit`            | Total budget for the current 10-second window |
| `X-Ratelimit-Remaining`        | How many total requests you have left         |
| `X-Ratelimit-Reset`            | Seconds until the window resets               |
| `X-Ratelimit-Method-Limit`     | Per-method cap for this RPC                   |
| `X-Ratelimit-Method-Remaining` | How many of this method you have left         |

## Client-side browser limits

If you connect through a browser, it caps how many requests run in parallel to one host, so a busy frontend can throttle itself before Triton's limits ever apply. To avoid it:

* Reuse one connection per page instead of opening one per call.
* Batch reads with `getMultipleAccounts` instead of many parallel `getAccountInfo` calls.
* Move heavy reads (program scans, signature crawls) to your backend.

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-laptop">:laptop:</i> <strong>Platform overview</strong></td><td>Where to get your endpoint and token, monitor usage, and top up your balance.</td><td><a href="https://app.gitbook.com/s/ACym6ZbIwDBDKhyKgDGy/platform-overview">Platform overview</a></td></tr><tr><td><i class="fa-earth-americas">:earth-americas:</i> <strong>Endpoints, regions, and routing</strong></td><td>All available endpoints, regions they're served from, routing options, and how to use each</td><td><a href="https://app.gitbook.com/s/ACym6ZbIwDBDKhyKgDGy/endpoints-regions-routing">https://app.gitbook.com/s/ACym6ZbIwDBDKhyKgDGy/endpoints-regions-routing</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
