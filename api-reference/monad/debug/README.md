---
description: Pull raw, RLP-encoded chain data and step-by-step execution traces.
  The `debug_*` namespace: raw block/header/receipt/transaction access and
  transaction tracing.
---

# Debug & tracing

These give you the raw, RLP-encoded form of chain data:

| Method | What it does |
| ------ | ------------ |
| `debug_getRawBlock` | The raw, RLP-encoded block. |
| `debug_getRawHeader` | The raw, RLP-encoded block header. |
| `debug_getRawReceipts` | The raw, RLP-encoded receipts of a block. |
| `debug_getRawTransaction` | The raw, RLP-encoded transaction. |

These replay execution so you can see exactly what ran:

| Method | What it does |
| ------ | ------------ |
| `debug_traceBlockByHash` | Execution traces for a block by hash. |
| `debug_traceBlockByNumber` | Execution traces for a block by number. |
| `debug_traceTransaction` | Execution traces for a transaction. |
| `debug_traceCall` | Execution traces for a call against a block. |

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
