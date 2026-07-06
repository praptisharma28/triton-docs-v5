---
description: Patterns for landing Solana transactions reliably under congestion.
---

# Best practices

## Build and sign correctly

* **Use a recent blockhash from `confirmed` or `finalized` commitment.** Never use `processed` for blockhashes.
* **Set a tight compute-unit budget.** The default is often too high; an accurate budget helps validators pack your transaction (a simple SOL transfer uses \~500 CUs).

## Price priority fees with percentiles

* **Don't use the default `getRecentPrioritizationFees` for fee estimation.** It returns only the minimum paid in recent blocks, which is often zero.
* **Pass the `percentile` parameter** (an integer 1 to 10,000, where `5000` is the median) to get a realistic market rate for the accounts your transaction write-locks.

## Simulate, then send without preflight

* **Simulate with `simulateTransaction()`, then send with `skipPreflight: true`.** Triton's send path has more delivery pathways for sends that skip preflight, and `skipPreflight: false` bills your send twice.

## Route through staked validators (SWQoS)

* **Nothing to enable: SWQoS is applied on every Triton endpoint by default, at no extra cost.** Your sends travel the reserved private connection pools of staked validators instead of the congested public TPU ports, raising delivery success.

## Handle retries yourself

* **SWQoS transaction bandwidth is free and enabled by default on every endpoint** — no need to request it. Your transactions go over the reserved private connection pools of staked validators, bypassing the congested public TPU ports and raising delivery success.
* Combine with priority fees, or Jito, for more speed during contention.

## Use `/sendtx` for latency-sensitive sends

* **Prefer `POST /sendtx` over JSON-RPC `sendTransaction` when latency matters.** It removes the JSON-RPC envelope, JSON parsing, and CORS overhead. It is submission only; simulate through the normal RPC interface.
* **Send with `Content-Type: application/octet-stream` or `text/plain`** so browsers skip the preflight `OPTIONS` round-trip.
* **Set `max_retries` above 0** (for example `3`). Unlike `sendTransaction`, `/sendtx` retries only according to this parameter, and leaving it at 0 can drop your landing rate.
* **Track signatures client-side** (the signature is deterministic and derivable before sending); request `response=signature` only if you need it back.

## Confirm before you trust

* **Treat a transaction as landed only at `confirmed` or `finalized`.** A `processed` transaction can still be dropped during minority forks or congestion.

## Protect transactions with Yellowstone Shield

* **Apply forwarding policies** (allowlist or blocklist of validators) to avoid sandwich and front-running validators. Pass them via `forwardingPolicies` in `sendTransaction` or the `Solana-ForwardingPolicies` header.
* **Shield only works on Shield-enabled RPCs** (those using Jet sender); standard Solana RPCs ignore the policy.
* **Maintain your lists every epoch** and treat Shield as a filter, not a guarantee. Be careful with strict policies on time-critical sends (arbitrage, liquidations): Shield drops, rather than queues, a transaction when no eligible validator is available.

## Simulate Jito bundles on Triton

* **Use `simulateBundle`** on Triton's Jito-enabled RPC, available on all plans.
* **Submitting (`sendBundle`) is Jito-exclusive:** it goes to Jito's Block Engine and requires your IP to be whitelisted there.

## Stay within limits and reuse connections

* **On HTTP `429`, pause all requests from that IP for the full 10-second window** before retrying.
* **For high-throughput senders, reuse a pooled HTTP connection** (a global undici / `https.Agent`, \~50 to start) to avoid ephemeral-port exhaustion. SDKs like `jito-ts` (`node-fetch`) hit the same issue; cap connections with an agent.
* **Run backend sending bots on a dedicated node**, not the shared service.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
