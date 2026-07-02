---
description: Submit signed transactions, simulate them first, and size the fees to land them.
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

# Sending transactions

These submit and simulate transactions:

| Method | What it does |
| ------ | ------------ |
| `sendTx` | Direct HTTP submission endpoint that bypasses JSON-RPC for the lowest submission latency. |
| `sendTransaction` | Submits a signed transaction over JSON-RPC and returns its signature. |
| `simulateTransaction` | Runs a signed transaction against current chain data without sending it. |

These size the fee before you send:

| Method | What it does |
| ------ | ------------ |
| `getFeeForMessage` | The fee the cluster would charge to process a message. |
| `getRecentPrioritizationFees` | Recent priority-fee samples for setting a competitive fee. |

To build swap transactions, see the [Metis](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/sending-transactions/metis) and [Titan](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/sending-transactions/titan) swap APIs.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
