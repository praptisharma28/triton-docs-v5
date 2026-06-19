---
description: Query epochs, slots, leaders, blockhashes, supply, inflation, and node health.
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

# Utility methods

These track where the cluster is in its epoch and slot schedule:

| Method | What it returns |
| ------ | --------------- |
| `getEpochInfo` | The current epoch position and counters. |
| `getEpochSchedule` | The cluster's epoch schedule parameters. |
| `getSlot` | The highest slot reached at the requested commitment. |
| `getSlotLeader` | The validator identity scheduled for the current slot. |
| `getSlotLeaders` | The validator identity for each slot in a range. |
| `getLeaderSchedule` | The leader schedule for the epoch containing a slot. |

These give you a blockhash to sign with and check whether one is still good:

| Method | What it returns |
| ------ | --------------- |
| `getLatestBlockhash` | The latest blockhash and its last valid block height. |
| `isBlockhashValid` | Whether a blockhash is still valid. |

These report how far this node's ledger reaches:

| Method | What it returns |
| ------ | --------------- |
| `getMaxRetransmitSlot` | The highest slot this node has seen in retransmit. |
| `getMaxShredInsertSlot` | The highest slot for which shreds were inserted. |
| `minimumLedgerSlot` | The lowest slot still in this node's local ledger. |

These identify the node and the cluster it is connected to:

| Method | What it returns |
| ------ | --------------- |
| `getClusterNodes` | The network endpoints, version, and feature set of known nodes. |
| `getIdentity` | This RPC node's identity public key. |
| `getVersion` | The node's software version. |
| `getHealth` | Whether this RPC node is healthy. |
| `getGenesisHash` | The genesis hash of the connected cluster. |
| `getHighestSnapshotSlot` | The highest full and incremental snapshot slots. |

These read token supply, inflation, and test-cluster airdrops:

| Method | What it returns |
| ------ | --------------- |
| `getSupply` | The total, circulating, and non-circulating supply. |
| `getInflationGovernor` | The inflation governor parameters. |
| `getInflationRate` | The current epoch's effective inflation rates. |
| `getInflationReward` | The inflation reward credited to addresses for an epoch. |
| `requestAirdrop` | Funds an account with lamports on test clusters only. |

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
