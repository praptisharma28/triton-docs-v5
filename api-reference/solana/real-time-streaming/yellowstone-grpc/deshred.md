---
description: >-
  Stream pre-execution (deshred) transactions over gRPC, before they are
  executed.
---

# Deshred transactions

## Filter

`SubscribeRequestFilterDeshredTransactions`, keyed by a filter label:

**`vote`** · optional bool: Include vote transactions.

**`account_include`** · repeated string: Match transactions touching any of these accounts (static or ALT-loaded).

**`account_exclude`** · repeated string: Exclude transactions touching any of these accounts.

**`account_required`** · repeated string: Only match transactions touching all of these accounts.

## Subscribe request

{% tabs %}
{% tab title="grpcurl" %}
```bash
grpcurl \
  -d '{"deshred_transactions": {"client": {"account_include": ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v"]}}}' \
  -H "x-token: <your-token>" \
  <your-endpoint>.mainnet.rpcpool.com:443 \
  geyser.Geyser/SubscribeDeshred
```
{% endtab %}
{% endtabs %}

## Update

Each arrives as a `SubscribeUpdateDeshred` with `deshred_transaction` set:

**`transaction`** · SubscribeUpdateDeshredTransactionInfo: `signature`, `is_vote`, the `transaction`, resolved `loaded_writable_addresses` / `loaded_readonly_addresses`, and the completed-data-set shred indices.

**`slot`** · uint64: Slot the transaction belongs to.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
