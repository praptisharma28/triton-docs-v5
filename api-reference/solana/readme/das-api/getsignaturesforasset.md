---
description: >-
  This method retrieves the transaction signatures associated with a compressed
  digital asset/NFT. It can be identified by its ID or by tree and leaf index.
---

# getSignaturesForAsset

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": "123",
    "method": "getAssetSignatures",
    "params": {
      "id": "EZsmgudhX6EFVfrdxmShDiNj365E85x7kpLJCVvPLVSH"
    }
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Name      | Description                                    | Required       |
| --------- | ---------------------------------------------- | -------------- |
| id        | The unique identifier of the compressed asset. | or tree + leaf |
| tree      | The tree corresponding to the leaf.            | or id          |
| leafIndex | The leaf index of digital compressed asset.    | or id          |

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
