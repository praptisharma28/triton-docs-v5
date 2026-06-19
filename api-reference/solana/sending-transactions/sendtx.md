---
description: How to send Solana transactions reliably through Triton's SWQoS routing.
---

# sendTx

Reliable transaction delivery on Solana is a client-side strategy. These practices significantly increase your inclusion rate on Triton's SWQoS routing.

* **Handle retries in your own code.** Do not rely on the RPC node to retry for you. Build asynchronous retry logic, re-fetching a recent blockhash and re-signing every few seconds.
* **Set `maxRetries: 0`** in your `sendTransaction` options, so the node does not use its legacy retry queue.
* **Set `skipPreflight: true`.** If you need a simulation, run `simulateTransaction()` separately beforehand.
* **Use a `finalized` or `confirmed` blockhash.** Never use `processed` for blockhashes.
* **Set a tight compute-unit budget and a competitive priority fee.**

For the method itself, see [sendTransaction](sendtransaction.md). For percentile-based priority-fee estimates, see [getRecentPrioritizationFees](getrecentprioritizationfees.md).

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
