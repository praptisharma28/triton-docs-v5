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
