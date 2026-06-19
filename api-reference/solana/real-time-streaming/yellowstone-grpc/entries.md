---
description: Stream entries (PoH ticks and transaction batches) over Dragon's Mouth gRPC.
---

# Entry updates

## Filter

`SubscribeRequestFilterEntry` (no fields):

This filter takes no fields.

## Subscribe request

{% tabs %}
{% tab title="grpcurl" %}
```bash
grpcurl \
  -d '{"entry": {"all": {}}, "commitment": "PROCESSED"}' \
  -H "x-token: <your-token>" \
  <your-endpoint>.mainnet.rpcpool.com:443 \
  geyser.Geyser/Subscribe
```
{% endtab %}
{% endtabs %}

## Update

Each arrives as a `SubscribeUpdate` with `entry` set (`SubscribeUpdateEntry`):

**`slot`** · uint64: Slot the entry belongs to.

**`index`** · uint64: Entry index within the slot.

**`num_hashes`** · uint64: Number of PoH hashes since the previous entry.

**`hash`** · bytes: Entry hash.

**`executed_transaction_count`** · uint64: Transactions in this entry.

**`starting_transaction_index`** · uint64: Index of the first transaction in this entry within the block.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
