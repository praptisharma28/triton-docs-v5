---
description: Stream live chain updates over standard WebSockets or high-throughput gRPC.
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

# Real-time streaming

Two interfaces stream live updates; pick by transport and throughput:

| Interface | Best for |
| --------- | -------- |
| Whirligig WebSocket | Standard WebSocket subscriptions to accounts, programs, logs, signatures, slots, and roots. |
| gRPC (Dragon's Mouth) | High-throughput, backend-to-backend Geyser streaming of accounts, transactions, slots, blocks, and entries. |

On gRPC, [Deshred](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/real-time-streaming/yellowstone-grpc/deshred) streams transactions reconstructed from shreds before the validator executes them, for the earliest possible signal.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
