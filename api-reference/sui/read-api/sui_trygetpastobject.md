---
description: >-
  Note there is no software-level guarantee/SLA that objects with past versions
  can be retrieved by this API, even if the object and version exists/existed.
  The result may vary across nodes depending on.
---

# sui\_tryGetPastObject

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_tryGetPastObject",
  "params": [
    "0x11af4b844ff94b3fbef6e36b518da3ad4c5856fa686464524a876b463d129760",
    4,
    {
      "showType": true,
      "showOwner": true,
      "showPreviousTransaction": true,
      "showDisplay": false,
      "showContent": true,
      "showBcs": false,
      "showStorageRebate": true
    }
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter   | Type              | Required | Description                                                                     |
| ----------- | ----------------- | -------- | ------------------------------------------------------------------------------- |
| `object_id` | ObjectID          | Yes      | the ID of the queried object                                                    |
| `version`   | SequenceNumber2   | Yes      | the version of the queried object. If None, default to the latest known version |
| `options`   | ObjectDataOptions | No       | options for specifying the content to be returned                               |

## Response

Returns `SuiPastObjectResponse`. (`ObjectRead`)

```json
{
  "status": "VersionFound",
  "details": {
    "objectId": "0x11af4b844ff94b3fbef6e36b518da3ad4c5856fa686464524a876b463d129760",
    "version": "4",
    "digest": "5VPAwDXy3BL72ehFc7gSJoz27ahMd6spUg5YwYc4ibcv",
    "type": "0x2::coin::Coin<0x2::sui::SUI>",
    "owner": {
      "AddressOwner": "0x3568c40e814d9d5396d23087a0fd641e91e0e00df6c012cded9ef9ba5e5bf042"
    },
    "previousTransaction": "5jQByoouHBwaico5pQB73GdbzerC2StjTiHh5garBjiV",
    "storageRebate": "100",
    "content": {
      "dataType": "moveObject",
      "type": "0x2::coin::Coin<0x2::sui::SUI>",
      "hasPublicTransfer": true,
      "fields": {
        "balance": "10000",
        "id": {
          "id": "0x11af4b844ff94b3fbef6e36b518da3ad4c5856fa686464524a876b463d129760"
        }
      }
    }
  }
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
