---
description: Return a checkpoint
---

# sui\_getCheckpoint

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_getCheckpoint",
  "params": [
    "1000"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type         | Required | Description                                                                                      |
| --------- | ------------ | -------- | ------------------------------------------------------------------------------------------------ |
| `id`      | CheckpointId | Yes      | Checkpoint identifier, can use either checkpoint digest, or checkpoint sequence number as input. |

## Response

Returns `Checkpoint`.

```json
{
  "epoch": "5000",
  "sequenceNumber": "1000",
  "digest": "G6Dtzr1ZSfHFhotGsTE3cLENa7L1ooe1BBvknAUsARbV",
  "networkTotalTransactions": "792385",
  "previousDigest": "6tBy8RXZKrdrB4XkMQn7J3MNG4fQCo9XcRduFFvYrL5Z",
  "epochRollingGasCostSummary": {
    "computationCost": "0",
    "storageCost": "0",
    "storageRebate": "0",
    "nonRefundableStorageFee": "0"
  },
  "timestampMs": "1676911928",
  "transactions": [
    "mN8YNBgVR3wB7vfXmjVgDRF4oqxVRRjzmJ6U4mzbq77"
  ],
  "checkpointCommitments": [],
  "validatorSignature": "wAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA"
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
