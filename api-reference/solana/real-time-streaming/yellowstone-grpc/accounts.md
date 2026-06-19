---
description: >-
  Stream account writes over Dragon's Mouth gRPC by adding an accounts filter to
  your SubscribeRequest.
---

# Account updates

## Filter

`SubscribeRequestFilterAccounts`, keyed by a filter label:

**`account`** · repeated string: Account pubkeys to match, base-58 encoded.

**`owner`** · repeated string: Match accounts owned by these program pubkeys.

**`filters`** · repeated SubscribeRequestFilterAccountsFilter: Extra filters: `memcmp`, `datasize`, `token_account_state`, or `lamports`.

**`nonempty_txn_signature`** · optional bool: Only deliver updates that carry a transaction signature.

**`cuckoo_accounts_filter`** · optional CuckooFilter: Compressed cuckoo filter for very large account sets.

## Subscribe request

{% tabs %}
{% tab title="grpcurl" %}
```bash
grpcurl \
  -d '{"accounts": {"usdc": {"account": ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v"]}}, "commitment": "CONFIRMED"}' \
  -H "x-token: <your-token>" \
  <your-endpoint>.mainnet.rpcpool.com:443 \
  geyser.Geyser/Subscribe
```
{% endtab %}
{% endtabs %}

## Update

Each write arrives as a `SubscribeUpdate` with `account` set (`SubscribeUpdateAccount`):

**`account`** · SubscribeUpdateAccountInfo: The account: `pubkey`, `lamports`, `owner`, `executable`, `rent_epoch`, `data`, `write_version`, `txn_signature`.

**`slot`** · uint64: Slot at which the write occurred.

**`is_startup`** · bool: True if the update is part of the initial startup snapshot.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
