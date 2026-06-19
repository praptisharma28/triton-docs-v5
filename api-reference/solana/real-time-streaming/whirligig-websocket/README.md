---
description: Subscribe over WebSockets to live account, program, log, signature, and slot updates.
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

# Whirligig WebSockets

These push live updates for accounts and the programs that own them:

| Method | What it does |
| ------ | ------------ |
| `accountSubscribe` | Notifications when an account's lamports or data change. |
| `programSubscribe` | Notifications for accounts owned by a program. |

These follow transactions and the logs they emit:

| Method | What it does |
| ------ | ------------ |
| `logsSubscribe` | Transaction log messages matching a filter. |
| `signatureSubscribe` | Status notifications for a transaction signature. |
| `transactionSubscribe` | Transaction notifications (a Triton extension). |

These track the validator's slot and root progress:

| Method | What it does |
| ------ | ------------ |
| `slotSubscribe` | Notifications when the validator processes a new slot. |
| `rootSubscribe` | Notifications when the validator sets a new root. |

Each has a matching unsubscribe method that cancels it by id (`accountUnsubscribe`, `programUnsubscribe`, and so on).

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
