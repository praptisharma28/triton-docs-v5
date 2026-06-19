---
description: Stream lightweight transaction-status updates over Dragon's Mouth gRPC.
---

# Transaction status

## Subscribe request

{% tabs %}
{% tab title="grpcurl" %}
```bash
grpcurl \
  -d '{"transactions_status": {"client": {"account_include": ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v"]}}, "commitment": "CONFIRMED"}' \
  -H "x-token: <your-token>" \
  <your-endpoint>.mainnet.rpcpool.com:443 \
  geyser.Geyser/Subscribe
```
{% endtab %}
{% endtabs %}

## Update

Each arrives as a `SubscribeUpdate` with `transaction_status` set (`SubscribeUpdateTransactionStatus`):

**`slot`** · uint64: Slot the transaction was processed in.

**`signature`** · bytes: Transaction signature.

**`is_vote`** · bool: Whether it is a vote transaction.

**`index`** · uint64: Index within the block.

**`err`** · TransactionError: Error, if the transaction failed.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
