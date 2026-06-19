---
description: Read staking positions, validator returns, and system-level state for the network.

---

# Governance read API

These read an address's delegated staking positions:

| Method | What it returns |
| ------ | --------------- |
| `suix_getStakes` | All delegated stakes for an address. |
| `suix_getStakesByIds` | Delegated stakes looked up by their ids. |

These cover validators, the committee, and network-wide state:

| Method | What it returns |
| ------ | --------------- |
| `suix_getValidatorsApy` | The APY of each validator. |
| `suix_getCommitteeInfo` | The validator committee for an epoch. |
| `suix_getLatestSuiSystemState` | The latest on-chain system state. |
| `suix_getReferenceGasPrice` | The network's reference gas price. |

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
