---
description: >-
  Return the list of objects owned by an address. Note that if the address owns
  more than `QUERY_MAX_RESULT_LIMIT` objects, the pagination is not accurate,
  because previous page may have been updated wh
---

# suix\_getOwnedObjects

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_getOwnedObjects",
  "params": [
    "0xdbc9abc01a87906b033a75750e741edb2df5ea5d55c96a611371d22799d26827",
    {
      "filter": {
        "MatchAll": [
          {
            "StructType": "0x2::coin::Coin<0x2::sui::SUI>"
          },
          {
            "AddressOwner": "0xdbc9abc01a87906b033a75750e741edb2df5ea5d55c96a611371d22799d26827"
          },
          {
            "Version": "13488"
          }
        ]
      },
      "options": {
        "showType": true,
        "showOwner": true,
        "showPreviousTransaction": true,
        "showDisplay": false,
        "showContent": false,
        "showBcs": false,
        "showStorageRebate": false
      }
    },
    "0x0cd4bb4d4f520fe9bbf0cf1cebe3f2549412826c3c9261bff9786c240123749f",
    3
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type                | Required | Description                                                                                                                                                        |
| --------- | ------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `address` | SuiAddress          | Yes      | the owner's Sui address                                                                                                                                            |
| `query`   | ObjectResponseQuery | No       | the objects query criteria.                                                                                                                                        |
| `cursor`  | ObjectID            | No       | An optional paging cursor. If provided, the query will start from the next item after the specified cursor. Default to start from the first item if not specified. |
| `limit`   | integer             | No       | Max number of items returned per page, default to \[QUERY\_MAX\_RESULT\_LIMIT] if not specified.                                                                   |

## Response

Returns `ObjectsPage`. (`Page_for_SuiObjectResponse_and_ObjectID`)

```json
{
  "data": [
    {
      "data": {
        "objectId": "0x0b37a91692359a98496738a58c17a9334aeacc435c70ab9635e47a277d8f8dd9",
        "version": "13488",
        "digest": "FZzfCnKCSRW2jN9AwkiarjYQapViUQAh799aiRMZ4YC2",
        "type": "0x2::coin::Coin<0x2::sui::SUI>",
        "owner": {
          "AddressOwner": "0xdbc9abc01a87906b033a75750e741edb2df5ea5d55c96a611371d22799d26827"
        },
        "previousTransaction": "AJhAseKLEndWYT45FbvYGgCJQTqZP537xqNnthY9FqSa",
        "storageRebate": "100"
      }
    },
    {
      "data": {
        "objectId": "0xd4feace07fc863a2eef286c3e95ed48e2c181bb65db5beaf7ea664b4ca6b744c",
        "version": "13488",
        "digest": "3cxBDcfnkVgtXWhnMnKKkMGtZdiEorUhb1vdp2DkVyfi",
        "type": "0x2::coin::Coin<0x2::sui::SUI>",
        "owner": {
          "AddressOwner": "0xdbc9abc01a87906b033a75750e741edb2df5ea5d55c96a611371d22799d26827"
        },
        "previousTransaction": "8qCvxDHh5LtDfF95Ci9G7vvQN2P6y4v55S9xoKBYp7FM",
        "storageRebate": "100"
      }
    },
    {
      "data": {
        "objectId": "0xe26860fac6839ce2d7ed7e6f29d276a1b4c23f2d9a9b6f0d8b2c17beace292b7",
        "version": "13488",
        "digest": "3tX9sgYC4A6nVKGjKEE5xxW6t4zkvDL9BwjuaxMg8arP",
        "type": "0x2::coin::Coin<0x2::sui::SUI>",
        "owner": {
          "AddressOwner": "0xdbc9abc01a87906b033a75750e741edb2df5ea5d55c96a611371d22799d26827"
        },
        "previousTransaction": "5Ka3vDaDy9h5UYk3Maz3vssWHrhbcGXQgwg8fL2ygyTi",
        "storageRebate": "100"
      }
    }
  ],
  "nextCursor": "0xe26860fac6839ce2d7ed7e6f29d276a1b4c23f2d9a9b6f0d8b2c17beace292b7",
  "hasNextPage": true
}
```
