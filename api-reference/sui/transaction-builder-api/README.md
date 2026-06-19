---
description: Build common transactions server-side, returning an unsigned transaction ready to sign.
  Build unsigned transactions server-side with the unsafe_* methods. Most
  applications should build transactions with the Sui SDK instead.
---

# Transaction builder API

These build an unsigned transfer or payment that you then sign and submit:

| Method | What it does |
| ------ | ------------ |
| `unsafe_transferObject` | Builds an unsigned transfer of an object to an address. |
| `unsafe_transferSui` | Builds an unsigned transfer of SUI to an address. |
| `unsafe_pay` | Builds an unsigned payment of a coin type to several addresses. |
| `unsafe_paySui` | Builds an unsigned payment of SUI to several addresses. |
| `unsafe_payAllSui` | Builds an unsigned payment of all SUI to one address. |

These build unsigned operations on coin objects:

| Method | What it does |
| ------ | ------------ |
| `unsafe_splitCoin` | Builds an unsigned split of a coin into several coins. |
| `unsafe_splitCoinEqual` | Builds an unsigned split of a coin into equal parts. |
| `unsafe_mergeCoins` | Builds an unsigned merge of several coins into one. |

These build unsigned Move calls, package publishes, and batched operations:

| Method | What it does |
| ------ | ------------ |
| `unsafe_moveCall` | Builds an unsigned Move call. |
| `unsafe_publish` | Builds an unsigned package publish. |
| `unsafe_batchTransaction` | Builds an unsigned batch of several operations. |

These build unsigned staking operations:

| Method | What it does |
| ------ | ------------ |
| `unsafe_requestAddStake` | Builds an unsigned add-stake to a validator. |
| `unsafe_requestWithdrawStake` | Builds an unsigned withdraw-stake from a validator. |

Most applications build transactions with the Sui SDK and use these only when building server-side.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
