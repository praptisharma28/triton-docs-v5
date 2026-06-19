---
description: Stream block metadata (no transactions) over Dragon's Mouth gRPC.
---

# Block metadata

## Filter

`SubscribeRequestFilterBlocksMeta` (no fields):

This filter takes no fields.

## Subscribe request

{% tabs %}
{% tab title="grpcurl" %}
```bash
grpcurl \
  -d '{"blocks_meta": {"all": {}}, "commitment": "CONFIRMED"}' \
  -H "x-token: <your-token>" \
  <your-endpoint>.mainnet.rpcpool.com:443 \
  geyser.Geyser/Subscribe
```
{% endtab %}
{% endtabs %}

## Update

Each arrives as a `SubscribeUpdate` with `block_meta` set (`SubscribeUpdateBlockMeta`):

**`slot`** · uint64: Block slot.

**`blockhash`** · string: Block hash.

**`rewards`** · Rewards: Block rewards.

**`block_time`** · UnixTimestamp: Block time.

**`block_height`** · BlockHeight: Block height.

**`parent_slot / parent_blockhash`** · uint64 / string: Parent slot and hash.

**`executed_transaction_count`** · uint64: Number of executed transactions.

**`entries_count`** · uint64: Number of entries in the block.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
