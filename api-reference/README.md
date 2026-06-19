---
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

# Reading account state

Read the current on-chain state of Solana accounts at any commitment level.

These return an account's data directly, whether you want a single account, many in one call, just the SOL balance, or every account a program owns:

| Method | What it returns |
| ------ | --------------- |
| `getAccountInfo` | The state and metadata of one account, or null if it doesn't exist. |
| `getMultipleAccounts` | The same, for many addresses in one call, in request order. |
| `getBalance` | The lamport (SOL) balance of an address. |
| `getProgramAccounts` | Every account a program owns, with optional data or size filters. |

These cover SPL token balances, holders, and supply:

| Method | What it returns |
| ------ | --------------- |
| `getTokenAccountBalance` | The balance of a single SPL token account. |
| `getTokenAccountsByOwner` | The token accounts a wallet owns. |
| `getTokenAccountsByDelegate` | The token accounts a given delegate can act on. |
| `getTokenSupply` | A mint's total supply. |
| `getTokenLargestAccounts` | The 20 largest holders of a mint. |

These read cluster-wide and validator state rather than one account you name:

| Method | What it returns |
| ------ | --------------- |
| `getLargestAccounts` | The 20 largest accounts by SOL balance. |
| `getMinimumBalanceForRentExemption` | The lamports an account of a given size needs to be rent-exempt. |
| `getStakeMinimumDelegation` | The cluster's minimum stake delegation. |
| `getVoteAccounts` | Current and delinquent vote accounts. |

For NFTs, compressed assets, and token metadata, use the [Metaplex DAS API](solana/readme/das-api/README.md).

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
