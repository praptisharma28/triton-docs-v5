---
description: Stream accounts, transactions, slots, blocks, and entries over one gRPC connection.
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
    visible: false
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# gRPC

The Subscribe stream delivers these update types in one connection:

| Update | What it streams |
| ------ | --------------- |
| Account updates | Account writes as they happen. |
| Transaction updates | Transactions as they are processed. |
| Transaction status | Lightweight transaction-status notifications. |
| Slot updates | Slot progression. |
| Block updates | Full blocks. |
| Block metadata | Block headers without transactions. |
| Entry updates | Ledger entries. |
| Deshred transactions | Transactions reconstructed from shreds pre-execution. |

Alongside the stream, a set of [unary calls](unary-calls/README.md) answer one-shot requests with no open stream.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
