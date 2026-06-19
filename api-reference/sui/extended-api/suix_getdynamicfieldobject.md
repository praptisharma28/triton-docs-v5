---
description: Return the dynamic field object information for a specified object
---

# suix\_getDynamicFieldObject

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_getDynamicFieldObject",
  "params": [
    "0x3ddea0f8c3da994d9ead562ce76e36fdef6a382da344930c73d1298b0e9644b8",
    {
      "type": "0x0000000000000000000000000000000000000000000000000000000000000009::test::TestField",
      "value": "some_value"
    }
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter          | Type             | Required | Description                         |
| ------------------ | ---------------- | -------- | ----------------------------------- |
| `parent_object_id` | ObjectID         | Yes      | The ID of the queried parent object |
| `name`             | DynamicFieldName | Yes      | The Name of the dynamic field       |

## Response

Returns `SuiObjectResponse`.

```json
{
  "data": {
    "objectId": "0x3ddea0f8c3da994d9ead562ce76e36fdef6a382da344930c73d1298b0e9644b8",
    "version": "1",
    "digest": "Faiv4yqGR4HjAW8WhMN1NHHNStxXgP3u22dVPyvLad2z",
    "type": "0x0000000000000000000000000000000000000000000000000000000000000009::test::TestField",
    "owner": {
      "AddressOwner": "0x5ea6f7a348f4a7bd1a9ab069eb7f63865de3075cc5a4e62432f634b50fd2bb2b"
    },
    "previousTransaction": "5qTpesGST3v9NmMTkzV7HHNZRJh52BSqUTErc6L6XGm",
    "storageRebate": "100",
    "content": {
      "dataType": "moveObject",
      "type": "0x0000000000000000000000000000000000000000000000000000000000000009::test::TestField",
      "hasPublicTransfer": true,
      "fields": {}
    }
  }
}
```
