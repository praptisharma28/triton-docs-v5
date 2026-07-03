---
description: Make one-shot gRPC requests for blockhashes, slots, version, and connection health.
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

# Unary calls

These return the current chain position and blockhash state:

| Method | What it returns |
| ------ | --------------- |
| `getLatestBlockhash` | The latest blockhash and last valid block height. |
| `getBlockHeight` | The current block height. |
| `getSlot` | The current slot. |
| `isBlockhashValid` | Whether a blockhash is still valid. |

These check the service and the connection itself:

| Method | What it returns |
| ------ | --------------- |
| `getVersion` | The Yellowstone gRPC service version. |
| `ping` | Health-check the gRPC connection. |
| `subscribeReplayInfo` | The first slot available for replay on a subscription. |

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
