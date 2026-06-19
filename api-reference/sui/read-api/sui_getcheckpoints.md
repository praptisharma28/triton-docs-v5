---
description: Return paginated list of checkpoints
---

# sui\_getCheckpoints

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_getCheckpoints",
  "params": [
    "1004",
    4,
    false
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter          | Type                | Required | Description                                                                                                                                                        |
| ------------------ | ------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `cursor`           | BigInt\_for\_uint64 | No       | An optional paging cursor. If provided, the query will start from the next item after the specified cursor. Default to start from the first item if not specified. |
| `limit`            | integer             | No       | Maximum item returned per page, default to \[QUERY\_MAX\_RESULT\_LIMIT\_CHECKPOINTS] if not specified.                                                             |
| `descending_order` | boolean             | Yes      | query result ordering, default to false (ascending order), oldest record first.                                                                                    |

## Response

Returns `CheckpointPage`. (`Page_for_Checkpoint_and_BigInt_for_uint64`)

```json
{
  "data": [
    {
      "epoch": "5000",
      "sequenceNumber": "1005",
      "digest": "9zA7Q9Ka1ykvYjSQGhQCdCf32FZkcWNWx7L22JczXGsk",
      "networkTotalTransactions": "792385",
      "previousDigest": "8BLFxLTjWZ2KqaGc3FjR1o9aL6kbyYrmhuNfJLU1ehYt",
      "epochRollingGasCostSummary": {
        "computationCost": "0",
        "storageCost": "0",
        "storageRebate": "0",
        "nonRefundableStorageFee": "0"
      },
      "timestampMs": "1676911928",
      "transactions": [
        "7RudGLkQDBNJyqrptkrNU66Zd3pvq8MHVAHYz9WpBm59"
      ],
      "checkpointCommitments": [],
      "validatorSignature": "wAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA"
    },
    {
      "epoch": "5000",
      "sequenceNumber": "1006",
      "digest": "FAUWHyWacmb4Vg4QGi9a6gqeVb7ixAZiL73FaGd6WpoV",
      "networkTotalTransactions": "792385",
      "previousDigest": "6Pn25cieaE62AT6BwCeBoca13AGZuneucaaTGqt3gNCo",
      "epochRollingGasCostSummary": {
        "computationCost": "0",
        "storageCost": "0",
        "storageRebate": "0",
        "nonRefundableStorageFee": "0"
      },
      "timestampMs": "1676911928",
      "transactions": [
        "7r7tmP5hzgrusiN6cucFwfTveqDb7K75tMJ7oNCyoDmy"
      ],
      "checkpointCommitments": [],
      "validatorSignature": "wAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA"
    },
    {
      "epoch": "5000",
      "sequenceNumber": "1007",
      "digest": "B3mzC6gy87SomUQwPsmVY7mtwkZLxfm5WwNi3kKyEb3x",
      "networkTotalTransactions": "792385",
      "previousDigest": "CnHTfdUJr1UUqwXkYUhbQjXeM16xR33UR62jE72toCis",
      "epochRollingGasCostSummary": {
        "computationCost": "0",
        "storageCost": "0",
        "storageRebate": "0",
        "nonRefundableStorageFee": "0"
      },
      "timestampMs": "1676911928",
      "transactions": [
        "Gb1UDqhmKMzMJ5FL37kBqCcuy4TtBL2ay3qec8tEUBLj"
      ],
      "checkpointCommitments": [],
      "validatorSignature": "wAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA"
    },
    {
      "epoch": "5000",
      "sequenceNumber": "1008",
      "digest": "HunuJWKu7azBfS47rJTq9FHTMvUDNVo2SK4hQeh5brXp",
      "networkTotalTransactions": "792385",
      "previousDigest": "38fLUfuigyzLPEDrsmRhcQmhKtbEUohuFBP9NDcWBmFz",
      "epochRollingGasCostSummary": {
        "computationCost": "0",
        "storageCost": "0",
        "storageRebate": "0",
        "nonRefundableStorageFee": "0"
      },
      "timestampMs": "1676911928",
      "transactions": [
        "GWTS9QR7mjNz9fBWGkk4JZU3mrzMXrmj74uS59Cd5und"
      ],
      "checkpointCommitments": [],
      "validatorSignature": "wAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA"
    }
  ],
  "nextCursor": "1008",
  "hasNextPage": true
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
