---
description: Read objects, transactions, checkpoints, and chain metadata from a Sui full node.

---

# Read API

These fetch the current or a past version of an object, one at a time or in batches:

| Method | What it returns |
| ------ | --------------- |
| `sui_getObject` | The data for one object. |
| `sui_multiGetObjects` | The data for many objects in one call. |
| `sui_tryGetPastObject` | A past version of an object, if it is still available. |
| `sui_tryMultiGetPastObjects` | Past versions of many objects, where available. |

These read transactions and the events they emit:

| Method | What it returns |
| ------ | --------------- |
| `sui_getTransactionBlock` | A transaction response by digest. |
| `sui_multiGetTransactionBlocks` | Transaction responses for many digests. |
| `sui_getTotalTransactionBlocks` | The total number of transaction blocks the server knows about. |
| `sui_getEvents` | The events emitted by a transaction. |

These cover checkpoints and the node's view of the chain:

| Method | What it returns |
| ------ | --------------- |
| `sui_getCheckpoint` | One checkpoint. |
| `sui_getCheckpoints` | A paginated list of checkpoints. |
| `sui_getLatestCheckpointSequenceNumber` | The sequence number of the latest executed checkpoint. |
| `sui_getProtocolConfig` | The protocol config for a given version. |
| `sui_getChainIdentifier` | The chain identifier, the first four bytes of the genesis checkpoint digest. |
| `sui_verifyZkLoginSignature` | Whether a zkLogin signature is valid. |

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
