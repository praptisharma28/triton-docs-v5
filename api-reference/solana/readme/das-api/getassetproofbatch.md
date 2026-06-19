---
description: Get merkle proofs for compressed assets by their IDs.
---

# getAssetProofBatch

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": "123",
  "method": "getAssetProofs",
  "params": {
    "ids": [
      "EZsmgudhX6EFVfrdxmShDiNj365E85x7kpLJCVvPLVSH",
      "Hhxb7pNvwi76WTSqFjMX5yXjSso1S3agEJ9kQZP9NQeJ"
    ]
  }
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Name | Description                                 | Required |
| ---- | ------------------------------------------- | -------- |
| ids  | A set of asset IDs used to retrieve proofs. | Yes      |

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
