---
description: Return the object information for a specified object
---

# sui\_getObject

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_getObject",
  "params": [
    "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809",
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

| Parameter   | Type              | Required | Description                                       |
| ----------- | ----------------- | -------- | ------------------------------------------------- |
| `object_id` | ObjectID          | Yes      | the ID of the queried object                      |
| `options`   | ObjectDataOptions | No       | options for specifying the content to be returned |

## Response

Returns `SuiObjectResponse`.

```json
{
  "data": {
    "objectId": "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809",
    "version": "1",
    "digest": "33K5ZXJ3RyubvYaHuEnQ1QXmmbhgtrFwp199dnEbL4n7",
    "type": "0x2::coin::Coin<0x2::sui::SUI>",
    "owner": {
      "AddressOwner": "0xc8ec1d5b84dd6289e193b9f88de4a994358c9f856135236c3e75a925e1c77ac3"
    },
    "previousTransaction": "5PLgmQye6rraDYqpV3npV6H1cUXoJZgJh1dPCyRa3WCv",
    "storageRebate": "100",
    "content": {
      "dataType": "moveObject",
      "type": "0x2::coin::Coin<0x2::sui::SUI>",
      "hasPublicTransfer": true,
      "fields": {
        "balance": "100000000",
        "id": {
          "id": "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809"
        }
      }
    }
  }
}
```
