---
description: >-
  Returns recent 60-second performance samples in reverse slot order. Each
  sample includes the number of transactions and slots recorded in that time
  window.
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# getRecentPerformanceSamples

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getRecentPerformanceSamples",
  "params": [
    2
  ]
}'
```
{% endtab %}

{% tab title="Kit" %}
```typescript
import { createSolanaRpc } from "@solana/kit";

const rpc_url = "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>";
const rpc = createSolanaRpc(rpc_url);

let limit = 2;

let performanceSamples = await rpc.getRecentPerformanceSamples(limit).send();

console.log(performanceSamples);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import { Connection } from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

let limit = 2;

let performanceSamples = await connection.getRecentPerformanceSamples(limit);

console.log(performanceSamples);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use solana_client::nonblocking::rpc_client::RpcClient;
use solana_commitment_config::CommitmentConfig;

#[tokio::main]
async fn main() -> Result<()> {
    let client = RpcClient::new_with_commitment(
        String::from("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>"),
        CommitmentConfig::confirmed(),
    );

    let limit = 2;
    let performance_samples = client.get_recent_performance_samples(limit.into()).await?;

    println!("{:#?}", performance_samples);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Parameters

**`number of samples`** · usize · default `720`

Number of samples to return (maximum 720)

## Response

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "slot": 348125,
      "numTransactions": 126,
      "numSlots": 126,
      "samplePeriodSecs": 60,
      "numNonVoteTransactions": 1
    },
    {
      "slot": 347999,
      "numTransactions": 126,
      "numSlots": 126,
      "samplePeriodSecs": 60,
      "numNonVoteTransactions": 1
    }
  ],
  "id": 1
}
```

## Result

An array of performance sample objects containing:

**`slot`** · u64

Slot at which the sample was taken.

**`numTransactions`** · u64

Number of transactions processed during the sample period

**`numSlots`** · u64

Number of slots completed during the sample period

**`samplePeriodSecs`** · u16

Number of seconds in a sample window

**`numNonVoteTransactions`** · u64 | null

Number of non-vote transactions processed during the sample period

{% hint style="info" %}
`numNonVoteTransactions` is `null` for older performance samples that do not record it. To get a number of voting transactions, compute `numTransactions - numNonVoteTransactions` when this field is present.
{% endhint %}

***

_Adapted from the_ [_Solana `getRecentPerformanceSamples` reference_](https://solana.com/docs/rpc/http/getrecentperformancesamples)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
