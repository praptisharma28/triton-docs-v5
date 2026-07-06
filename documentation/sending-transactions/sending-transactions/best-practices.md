---
description: Patterns for landing Solana transactions reliably under congestion.
---

# Best practices

How to land Solana transactions reliably, especially under congestion.

## Build and sign correctly

* **Use a recent blockhash from `confirmed` or `finalized` commitment. Never use `processed` for blockhashes.**
* **Set a tight compute-unit budget.** The default is often too high; an accurate budget helps validators pack your transaction (a simple SOL transfer uses only \~500 CUs).
* **Include a competitive priority fee.** Check the prevailing market rate for the accounts your transaction write-locks.

## Handle retries yourself

* **Do not rely on the RPC node to retry.** Its legacy retry queue saturates under high traffic and causes widespread failures.
* **Always send with `maxRetries: 0`** so the node does not use that queue.
* **`/sendtx` handles retries differently:** unlike `sendTransaction` (where `maxRetries: 0` lets Jet fan out retries for you), `/sendtx` retries according to its `max_retries` parameter. Set it above 0 (for example `3`); leaving it at 0 can drop your landing rate.
* **Build your own asynchronous retry logic:** re-fetch a recent blockhash and re-sign every few seconds.

## Separate simulation from sending

* **Simulate first with `simulateTransaction()`, then send with `sendTransaction()` and `skipPreflight: true`.** Triton's send path has more delivery pathways for sends that skip pre-flight, and leaving `skipPreflight: false` bills your send twice.

## Price priority fees with percentiles

* **Don't use the default `getRecentPrioritizationFees`** for fee estimation. It returns only the minimum paid in recent blocks, which is often zero.
* **Use the percentile parameter** (an integer 1 to 10,000, where `5000` is the median) to get a realistic, actionable estimate from recent on-chain activity.

## Route through staked validators (SWQoS)

* **SWQoS transaction bandwidth is free and enabled by default on every endpoint** — no need to request it. Your transactions go over the reserved private connection pools of staked validators, bypassing the congested public TPU ports and raising delivery success.
* Combine with priority fees, or Jito, for more speed during contention.

## Use the direct submission endpoint for latency

* **For latency-sensitive sends, use `POST /sendtx`** instead of JSON-RPC `sendTransaction`. It removes the JSON-RPC envelope, JSON parsing, and CORS overhead.
* **Send with `Content-Type: application/octet-stream` or `text/plain`** so browsers skip the preflight `OPTIONS` round-trip.
* **Track signatures client-side** (the signature is deterministic and derivable before sending); request `response=signature` only if you need it back.
* `/sendtx` is submission only. Use the normal RPC interface for simulation and for full `sendTransaction` options.

## Confirm before you trust

* **Treat a transaction as landed only at `confirmed` or `finalized`.** A `processed` transaction can still be dropped during minority forks or congestion.

## Protect transactions with Yellowstone Shield

* **Apply forwarding policies** (allowlist or blocklist of validators) to avoid sandwich and front-running validators. Pass them via `forwardingPolicies` in `sendTransaction` or the `Solana-ForwardingPolicies` header.
* **Shield only works on Shield-enabled RPCs** (those using Jet sender); standard Solana RPCs ignore the policy.
* **Maintain your lists every epoch** and treat Shield as a filter, not a guarantee. Be careful with strict policies on time-critical sends (arbitrage, liquidations): Shield drops, rather than queues, a transaction when no eligible validator is available.

## Jito Bundle Simulation

* **Simulate bundles with `simulateBundle`** (Jito RPC is available on all plans).
* **`sendBundle` is Jito-exclusive** and requires your IP to be whitelisted with Jito's Block Engine.

## Stay within limits and reuse connections

* **On HTTP `429`, pause all requests from that IP for the full 10-second window** before retrying.
* **For high-throughput senders, reuse a pooled HTTP connection** (a global undici / `https.Agent`, \~50 to start) to avoid ephemeral-port exhaustion. SDKs like `jito-ts` (`node-fetch`) hit the same issue; cap connections with an agent.
* **Run backend sending bots on a dedicated node**, not the shared service.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
