---
description: >-
  Fetches the proof path for a given compressed asset/NFT. This method is
  required for submitting changes to a compressed merkle tree which requires the
  proof path for the tree it belongs to.
---

# getAssetProof

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": "123",
  "method": "getAssetProof",
  "params": {
    "id": "EZsmgudhX6EFVfrdxmShDiNj365E85x7kpLJCVvPLVSH"
  }
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Name | Description                                    | Required |
| ---- | ---------------------------------------------- | -------- |
| id   | The unique identifier of the compressed asset. | Yes      |

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
