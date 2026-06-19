---
description: Look up confirmed blocks, transactions, and signatures from the chain's history.
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

# Historical data

These read confirmed blocks and the slots around them:

| Method | What it returns |
| ------ | --------------- |
| `getBlock` | A confirmed block for a slot, with its metadata and transactions. |
| `getBlocks` | The confirmed block slots in an inclusive range. |
| `getBlocksWithLimit` | Up to a limit of confirmed block slots from a start slot. |
| `getBlockTime` | The estimated production time of a block. |
| `getBlockHeight` | The block height at the requested commitment. |
| `getBlockProduction` | Block-production counts per validator identity. |
| `getBlockCommitment` | The stake-weighted commitment recorded for a slot. |
| `getFirstAvailableBlock` | The lowest confirmed block slot still in this node's ledger. |

These look up transactions and the signatures that touch an address:

| Method | What it returns |
| ------ | --------------- |
| `getTransaction` | A confirmed transaction by signature, or null if not found. |
| `getTransactionCount` | The total transactions the cluster has processed. |
| `getSignaturesForAddress` | The confirmed signatures that reference an address. |
| `getSignatureStatuses` | The current status of each supplied signature. |
| `getTransactionsForAddress` | An address's full history in one call, with server-side filters and pagination (a Triton extension). |

This samples recent network throughput:

| Method | What it returns |
| ------ | --------------- |
| `getRecentPerformanceSamples` | Recent 60-second network performance samples. |

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
