---
description: Transaction fee history
---

# eth\_feeHistory

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_feeHistory",
  "params": [
    "0x5",
    "latest",
    [
      20,
      30
    ]
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter           | Type              | Required | Description                                                                                                                                                                                                                                                                      |
| ------------------- | ----------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `blockCount`        | uint              | Yes      | Requested range of blocks. Clients will return less than the requested range if not all blocks are available.                                                                                                                                                                    |
| `newestBlock`       | BlockNumberOrTag  | Yes      | Highest block of the requested range.                                                                                                                                                                                                                                            |
| `rewardPercentiles` | rewardPercentiles | Yes      | A monotonically increasing list of percentile values. For each block in the requested range, the transactions will be sorted in ascending order by effective tip per gas and the corresponding effective tip for the percentile will be determined, accounting for gas consumed. |

## Response

Returns `Fee history result`.

```json
{
  "oldestblock": "0x10b52f",
  "baseFeePerGas": [
    "0x3fa63a3f",
    "0x37f999ee",
    "0x3e36f20a",
    "0x4099f79a",
    "0x430d532d",
    "0x46fcd4a4"
  ],
  "baseFeePerBlobGas": [
    "0x7b7609c19",
    "0x6dbe41789",
    "0x7223341d4",
    "0x6574a002c",
    "0x7223341d4",
    "0x6574a002c"
  ],
  "gasUsedRatio": [
    0.017712333333333333,
    0.9458865666666667,
    0.6534561,
    0.6517375666666667,
    0.7347769666666667
  ],
  "blobGasUsedRatio": [
    0.0,
    0.6666666666666666,
    0.0,
    1.0,
    0.0
  ],
  "reward": [
    [
      "0x3b9aca00",
      "0x59682f00"
    ],
    [
      "0x3a13012",
      "0x3a13012"
    ],
    [
      "0x3a13012",
      "0x3a13012"
    ],
    [
      "0xf4240",
      "0xf4240"
    ],
    [
      "0xf4240",
      "0xf4240"
    ]
  ]
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
