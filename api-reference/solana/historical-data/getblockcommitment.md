---
description: Returns the stake-weighted commitment information recorded for a slot.
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

# getBlockCommitment

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getBlockCommitment",
  "params": [
    5
  ]
}'
```
{% endtab %}

{% tab title="Kit" %}
```typescript
import { createSolanaRpc } from "@solana/kit";

const rpc_url = "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>";
const rpc = createSolanaRpc(rpc_url);

const slot_number = BigInt(5);

let blockCommitment = await rpc.getBlockCommitment(slot_number).send();

console.log("block commitment:", blockCommitment);
```
{% endtab %}
{% endtabs %}

## Parameters

**`slot number`** · u64 · required

Slot to query.

## Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "commitment": [
      0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
      0, 0, 0, 0, 0, 10, 32
    ],
    "totalStake": 42
  },
  "id": 1
}
```

## Result

Commitment response object containing:

**`commitment`** · array | null

Array of stake totals, in lamports, that voted on the block at each lockout depth from `0` through [`MAX_LOCKOUT_HISTORY`](https://github.com/anza-xyz/solana-sdk/blob/clock%40v2.2.3/vote-interface/src/state/mod.rs#L33).

**`totalStake`** · u64

Total active stake, in lamports, of the current epoch.

***

_Adapted from the_ [_Solana `getBlockCommitment` reference_](https://solana.com/docs/rpc/http/getblockcommitment)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
