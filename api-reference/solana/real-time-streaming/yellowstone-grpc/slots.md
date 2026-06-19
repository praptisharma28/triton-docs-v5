---
description: Stream slot status updates over Dragon's Mouth gRPC.
---

# Slot updates

## Filter

`SubscribeRequestFilterSlots`, keyed by a filter label:

**`filter_by_commitment`** · optional bool: Only deliver slots at the requested commitment.

**`interslot_updates`** · optional bool: Deliver intra-slot status updates (first-shred, completed, etc.).

## Subscribe request

{% tabs %}
{% tab title="grpcurl" %}
```bash
grpcurl \
  -d '{"slots": {"all": {}}, "commitment": "PROCESSED"}' \
  -H "x-token: <your-token>" \
  <your-endpoint>.mainnet.rpcpool.com:443 \
  geyser.Geyser/Subscribe
```
{% endtab %}
{% endtabs %}

## Update

Each arrives as a `SubscribeUpdate` with `slot` set (`SubscribeUpdateSlot`):

**`slot`** · uint64: The slot number.

**`parent`** · optional uint64: Parent slot.

**`status`** · SlotStatus: `PROCESSED`, `CONFIRMED`, `FINALIZED`, `FIRST_SHRED_RECEIVED`, `COMPLETED`, `CREATED_BANK`, or `DEAD`.

**`dead_error`** · optional string: Reason, if the slot was marked dead.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
