---
description: >-
  Price priority fees against the real market rate with a percentile parameter
  on getRecentPrioritizationFees, instead of the minimum fee.
---

# Priority fees API

Triton extends Solana's `getRecentPrioritizationFees` with a `percentile` parameter, so you can price priority fees against the real market rate instead of the minimum.

## The problem with standard priority fees

Solana's default `getRecentPrioritizationFees` returns only the **minimum** priority fee paid in recent blocks. That figure is often zero, so it is a poor signal for the fee a transaction actually needs to land.

## Percentile-based fees

Triton's `getRecentPrioritizationFees` accepts a `percentile` parameter. Request a percentile (for example the 50th for the median, or the 90th for a high-end fee) to get an actionable estimate from recent on-chain activity.

The method name is unchanged. `percentile` is an integer from 1 to 10,000, representing 0.01% to 100.00%, so `5000` is the 50th percentile (median). If you omit it, the method falls back to the standard behaviour and returns the minimum fee.

## Request

```json
{
  "jsonrpc": "2.0",
  "id": "1",
  "method": "getRecentPrioritizationFees",
  "params": [
    ["RNXnAJV1DeBt6Lytjz4wYzvS3d6bhsfidS5Np4ovwZz"],
    { "percentile": 5000 }
  ]
}
```

## Response

The response format matches the standard Solana RPC method, but `prioritizationFee` reflects the requested percentile instead of the minimum.

```json
{
  "jsonrpc": "2.0",
  "result": [
    { "slot": 348126, "prioritizationFee": 1000 },
    { "slot": 348127, "prioritizationFee": 500 }
  ],
  "id": 1
}
```

## Client integration

This is an enhanced method, so it may not be in standard SDKs. The [example repository](https://github.com/rpcpool/solana-prioritization-fees-api/) has utility functions to drop into a JavaScript or TypeScript codebase.

{% hint style="info" %}
Running your own nodes or another RPC service? Triton publishes the patches that enable percentile fees, so you can adopt the same behaviour: the [`v1.17.23-getrpf` branch](https://github.com/rpcpool/solana-public/tree/v1.17.23-getrpf) of `rpcpool/solana-public`, plus Agave [issue #3332](https://github.com/anza-xyz/agave/issues/3332) and [PR #217](https://github.com/anza-xyz/agave/pull/217).
{% endhint %}

## Pricing

The Priority fees API is billed as standard RPC: `$0.08 / GB` of bandwidth plus `$10 / million` calls.

## Resources

* Client examples: [rpcpool/solana-prioritization-fees-api](https://github.com/rpcpool/solana-prioritization-fees-api)

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-play">:play:</i> <strong>Quickstart</strong></td><td>Send a transaction with /sendtx in a few minutes.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/quickstart">Quickstart</a></td></tr><tr><td><i class="fa-list-check">:list-check:</i> <strong>Best practices</strong></td><td>Land more transactions, faster.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/best-practices">Best practices</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
