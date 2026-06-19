---
description: Call Monad over the standard Ethereum JSON-RPC interface, which is EVM-equivalent.
  The standard Ethereum `eth_*`, `net_*`, and `web3_*` methods, served by Triton
  on Monad.
---

# Ethereum JSON-RPC

These tell you about the chain and what it costs to transact on it:

| Method | What it does |
| ------ | ------------ |
| `eth_blockNumber` | The latest block number. |
| `eth_chainId` | The chain id. |
| `eth_gasPrice` | The current gas price in wei. |
| `eth_maxPriorityFeePerGas` | The current max priority fee per gas. |
| `eth_feeHistory` | Historical fee data. |
| `eth_syncing` | Sync status, or false if synced. |
| `net_version` | The current network id. |
| `web3_clientVersion` | The client version. |

These read the on-chain state at an address:

| Method | What it does |
| ------ | ------------ |
| `eth_getBalance` | The balance of an address. |
| `eth_getCode` | The code at an address. |
| `eth_getStorageAt` | The value at a storage slot. |
| `eth_getTransactionCount` | The nonce of an account. |

These run transactions and work out their gas before you send anything:

| Method | What it does |
| ------ | ------------ |
| `eth_call` | Executes a call without creating a transaction. |
| `eth_estimateGas` | Estimates the gas a transaction needs. |
| `eth_createAccessList` | Generates an access list for a transaction. |
| `eth_fillTransaction` | Fills in defaults such as gas, nonce, and fees for a transaction. |

These fetch blocks, transactions, and logs, and submit signed transactions:

| Method | What it does |
| ------ | ------------ |
| `eth_getBlockByHash` | A block by hash. |
| `eth_getBlockByNumber` | A block by number. |
| `eth_getBlockReceipts` | The receipts of a block. |
| `eth_getBlockTransactionCountByHash` | The transaction count of a block by hash. |
| `eth_getBlockTransactionCountByNumber` | The transaction count of a block by number. |
| `eth_getTransactionByHash` | A transaction by hash. |
| `eth_getTransactionByBlockHashAndIndex` | A transaction by block hash and index. |
| `eth_getTransactionByBlockNumberAndIndex` | A transaction by block number and index. |
| `eth_getTransactionReceipt` | The receipt of a transaction. |
| `eth_getLogs` | The logs matching a filter. |
| `eth_sendRawTransaction` | Submits a signed raw transaction. |

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
