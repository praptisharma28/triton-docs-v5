---
description: >-
  Note there is no software-level guarantee/SLA that objects with past versions
  can be retrieved by this API, even if the object and version exists/existed.
  The result may vary across nodes depending on.
---

# sui\_tryMultiGetPastObjects

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_tryMultiGetPastObjects",
  "params": [
    [
      {
        "objectId": "0x38b3186a7bb26a1ab2c982a0a9b482aa70f5a010fffc60f20194ef0f597474e8",
        "version": "4"
      },
      {
        "objectId": "0xceaf9ee4582d3a233101e322a22cb2a5bea2e681ea5af4e59bd1abb0bb4fcb27",
        "version": "12"
      }
    ],
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

| Parameter      | Type              | Required | Description                                       |
| -------------- | ----------------- | -------- | ------------------------------------------------- |
| `past_objects` | array             | Yes      | a vector of object and versions to be queried     |
| `options`      | ObjectDataOptions | No       | options for specifying the content to be returned |

## Response

Returns `Vec<SuiPastObjectResponse>`. (`array`)

```json
[
  {
    "status": "VersionFound",
    "details": {
      "objectId": "0x38b3186a7bb26a1ab2c982a0a9b482aa70f5a010fffc60f20194ef0f597474e8",
      "version": "4",
      "digest": "hvBGBXvKVhC7XYgVPujuiLjxASR6UGAkSFrCRtVxX1F",
      "type": "0x2::coin::Coin<0x2::sui::SUI>",
      "owner": {
        "AddressOwner": "0x47866ff92885a3c21a7703f564721c198308aa0c71b771ada6b96c16fc9c0fa7"
      },
      "previousTransaction": "6heEteheiLZcS8iVNXsNUnU7oVjzT7UHYzprGcuWQ4gG",
      "storageRebate": "100",
      "content": {
        "dataType": "moveObject",
        "type": "0x2::coin::Coin<0x2::sui::SUI>",
        "hasPublicTransfer": true,
        "fields": {
          "balance": "10000",
          "id": {
            "id": "0x38b3186a7bb26a1ab2c982a0a9b482aa70f5a010fffc60f20194ef0f597474e8"
          }
        }
      }
    }
  },
  {
    "status": "VersionFound",
    "details": {
      "objectId": "0xceaf9ee4582d3a233101e322a22cb2a5bea2e681ea5af4e59bd1abb0bb4fcb27",
      "version": "12",
      "digest": "B5z4YkAgTi78fdxMbxG3fv2V4YBkhpc8PRCPz8MzLtbf",
      "type": "0x2::coin::Coin<0x2::sui::SUI>",
      "owner": {
        "AddressOwner": "0xa6ced287081357950315a8842c3870f2d83f980fe0996a92d351d6749a0a0b47"
      },
      "previousTransaction": "BLN2oUCHmwmaAXXCxbojTcozUqZYfvXx4Bkgi7xcgyVc",
      "storageRebate": "100",
      "content": {
        "dataType": "moveObject",
        "type": "0x2::coin::Coin<0x2::sui::SUI>",
        "hasPublicTransfer": true,
        "fields": {
          "balance": "20000",
          "id": {
            "id": "0x38b3186a7bb26a1ab2c982a0a9b482aa70f5a010fffc60f20194ef0f597474e8"
          }
        }
      }
    }
  }
]
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
