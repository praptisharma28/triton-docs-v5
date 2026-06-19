---
description: Read coin balances, coin objects, and coin metadata for any address.

---

# Coin query API

These read an address's balances across one or every coin type:

| Method | What it returns |
| ------ | --------------- |
| `suix_getBalance` | The balance of one coin type for an address. |
| `suix_getAllBalances` | The balance of every coin type for an address. |

These return the underlying coin objects an address holds:

| Method | What it returns |
| ------ | --------------- |
| `suix_getCoins` | The coin objects of one type owned by an address. |
| `suix_getAllCoins` | Every coin object owned by an address. |

These read a coin type's own properties:

| Method | What it returns |
| ------ | --------------- |
| `suix_getCoinMetadata` | A coin's metadata, such as its symbol and decimals. |
| `suix_getTotalSupply` | The total supply of a coin. |

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
