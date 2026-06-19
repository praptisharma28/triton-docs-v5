---
description: Return transaction events.
---

# sui\_getEvents

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_getEvents",
  "params": [
    "11a72GCQ5hGNpWGh2QhQkkusTEGS6EDqifJqxr7nSYX"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter            | Type              | Required | Description               |
| -------------------- | ----------------- | -------- | ------------------------- |
| `transaction_digest` | TransactionDigest | Yes      | the event query criteria. |

## Response

Returns `Vec<SuiEvent>`. (`array`)

```json
{
  "data": [
    {
      "id": {
        "txDigest": "11a72GCQ5hGNpWGh2QhQkkusTEGS6EDqifJqxr7nSYX",
        "eventSeq": "0"
      },
      "packageId": "0xc54ab30a3d9adc07c1429c4d6bbecaf9457c9af77a91f631760853934d383634",
      "transactionModule": "test_module",
      "sender": "0xbcf7c32655009a61f1de0eae420a2e4ae1bb772ab2dd5d5a7dfa949c0ef06908",
      "type": "0x0000000000000000000000000000000000000000000000000000000000000009::test::TestEvent",
      "parsedJson": {
        "test": "example value"
      },
      "bcsEncoding": "base64",
      "bcs": ""
    }
  ],
  "nextCursor": {
    "txDigest": "11a72GCQ5hGNpWGh2QhQkkusTEGS6EDqifJqxr7nSYX",
    "eventSeq": "5"
  },
  "hasNextPage": false
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
