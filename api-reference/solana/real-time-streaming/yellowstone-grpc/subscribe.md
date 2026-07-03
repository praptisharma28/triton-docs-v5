---
description: >-
  Open a Dragon's Mouth gRPC stream and subscribe to one or more data types in a
  single request.
---

# Subscribe

`Subscribe` is a single bidirectional gRPC stream. You send one `SubscribeRequest` describing every data type you want, and the server streams back matching `SubscribeUpdate` messages until you close the connection. You can update the request at any time by sending a new one on the same stream.

## Request

A `SubscribeRequest` carries one map per data type, each keyed by a label you choose, plus stream-wide options:

**`accounts`** · map: see [Account updates](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/real-time-streaming/yellowstone-grpc/accounts)

**`transactions`** · map: see [Transaction updates](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/real-time-streaming/yellowstone-grpc/transactions)

**`transactions_status`** · map: see [Transaction status](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/real-time-streaming/yellowstone-grpc/transaction-status)

**`slots`** · map: see [Slot updates](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/real-time-streaming/yellowstone-grpc/slots)

**`blocks`** · map: see [Block updates](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/real-time-streaming/yellowstone-grpc/blocks)

**`blocks_meta`** · map: see [Block metadata](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/real-time-streaming/yellowstone-grpc/blocks-meta)

**`entry`** · map: see [Entry updates](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/real-time-streaming/yellowstone-grpc/entries)

**`commitment`** · optional CommitmentLevel: `PROCESSED`, `CONFIRMED`, or `FINALIZED`.

**`accounts_data_slice`** · repeated: return only a slice (`offset`, `length`) of each account's data.

**`from_slot`** · optional uint64: replay from this slot before going live (subject to the replay window, see subscribeReplayInfo).

**`ping`** · optional: in-stream keepalive. Send `SubscribeRequestPing { id }` and the server returns a `SubscribeUpdatePong { id }` on the same stream. This is the keepalive ping, distinct from the standalone unary `ping`.

## Example

{% tabs %}
{% tab title="grpcurl" %}
```bash
grpcurl \
  -d '{"slots": {"all": {}}, "accounts": {"usdc": {"account": ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v"]}}, "commitment": "CONFIRMED"}' \
  -H "x-token: <your-token>" \
  <your-endpoint>.mainnet.rpcpool.com:443 \
  geyser.Geyser/Subscribe
```
{% endtab %}
{% endtabs %}

## Update

Every message is a `SubscribeUpdate` carrying `filters` (the labels that matched) and exactly one of the type payloads above, plus `created_at`. Subscribe to a specific type to see its shape.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
