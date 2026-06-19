---
description: Query owned objects, dynamic fields, events, names, and live streams beyond the core reads.
---

# Extended API

These list what an address or object owns:

| Method | What it returns |
| ------ | --------------- |
| `suix_getOwnedObjects` | The objects owned by an address. |
| `suix_getDynamicFields` | The dynamic fields owned by an object. |
| `suix_getDynamicFieldObject` | A specific dynamic field object. |

These run filtered queries over events and transactions:

| Method | What it returns |
| ------ | --------------- |
| `suix_queryEvents` | The events matching a query. |
| `suix_queryTransactionBlocks` | The transactions matching a query. |

These resolve SuiNS names to and from addresses:

| Method | What it returns |
| ------ | --------------- |
| `suix_resolveNameServiceAddress` | The address a SuiNS name resolves to. |
| `suix_resolveNameServiceNames` | The names that resolve to an address. |

These open a WebSocket stream of new activity as it lands:

| Method | What it returns |
| ------ | --------------- |
| `suix_subscribeEvent` | A stream of events over a WebSocket. |
| `suix_subscribeTransaction` | A stream of transaction effects over a WebSocket. |

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
