---
description: >-
  Questions about transaction landing, staked validators, and
  processed-vs-confirmed transaction lifecycle on Solana.
---

# Sending transactions

<details>

<summary>My transactions aren't landing successfully. What should I do?</summary>

Triton ensures reliable transaction delivery to the network, but for optimal transaction finalisation, we recommend configuring appropriate priority fees. See our [Priority Fees API](https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/priority-fees-api) for how to set them.

</details>

<details>

<summary>What is the advantage of sending transactions through a staked validator?</summary>

Staked validators have access to a much larger, reserved connection pool for sending transactions. During congestion, that's a significant advantage over the contested public connection pools. We provide stake-weighted QoS routing for free on every Triton subscription, contact support by clicking the chat icon in the bottom right of your [customer dashboard](https://customers.triton.one) to enable it on your endpoint.

</details>

<details>

<summary>Can a <code>processed</code> transaction on Solana still be dropped?</summary>

Yes. A transaction is only final once it reaches `confirmed` or `finalized` commitment. Slots containing `processed` transactions can still be dropped during minority forks or network congestion.

</details>

<details>

<summary>What's the difference between <code>sendtx</code> and <code>sendTransaction</code>?</summary>

Both route through the same open-source [Yellowstone Jet](https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/jet-sender) engine with SWQoS on by default, so the difference is the request format, not the delivery. `sendTransaction` is the standard JSON-RPC method: the server parses a JSON-RPC envelope before submitting. `/sendtx` is a direct HTTP endpoint: you POST the raw transaction bytes (or a base58/base64 string), which skips the envelope, the server-side JSON parsing, and the browser CORS preflight, and runs over HTTP/3 and QUIC.

</details>

<details>

<summary>Do I need to set up stake-weighted QoS (SWQoS)?</summary>

No. SWQoS routing applies by default to standard `sendTransaction` and `/sendtx` calls on your existing Solana mainnet endpoint, so there’s nothing to configure client-side.

</details>

<details>

<summary>Is there a minimum priority fee?</summary>

No. Solana's default `getRecentPrioritizationFees` returns the recent minimum, which is often zero, so use Triton's `percentile` parameter to set a fee that is competitive during congestion. See [Priority Fees API](https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/priority-fees-api).

</details>

<details>

<summary>Do you support sponsored transactions or gas sponsorship?</summary>

No. For transaction sending, Triton offers SWQoS delivery, [Yellowstone Shield](https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/sending-transactions/shield-mev-protection) validator policies, and priority-fee tooling.

</details>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
