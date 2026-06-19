---
description: Use Monad-specific methods for synchronous sends and transaction-pool inspection.

---

# Monad extensions

This submits a transaction and blocks until it lands:

| Method | What it does |
| ------ | ------------ |
| `eth_sendRawTransactionSync` | Submits a signed transaction and waits for its receipt. |

These let you inspect node activity and the pending pool:

| Method | What it does |
| ------ | ------------ |
| `admin_ethCallStatistics` | Statistics about eth_call execution on the node. |
| `txpool_statusByAddress` | The transaction-pool status for an address. |
| `txpool_statusByHash` | The transaction-pool status for a transaction hash. |

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
