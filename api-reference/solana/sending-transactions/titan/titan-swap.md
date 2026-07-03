---
description: Execute a swap from a streamed Titan quote.
---

# Titan /swap

Each Titan quote arrives with the individual Solana instructions needed to execute it, so you compose the swap with your own program logic. All routes are simulated on the same block against fresh on-chain data, so the quote reflects the real expected result rather than a theoretical best case.

## Flow

1. Connect to the Titan WebSocket (see [Titan quote](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/sending-transactions/titan/titan-quote)) and receive a live quote.
2. Take the swap instructions from the quote payload.
3. Build, sign, and send the transaction with your wallet.

For the instruction payload format and a TypeScript SDK (`@titanexchange/sdk-ts`), see the [Titan DART Swap API docs](https://titan-exchange.gitbook.io/titan/developer-doc/dart-swap-api/overview).

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
