---
description: Return the transaction response object.
---

# sui\_getTransactionBlock

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_getTransactionBlock",
  "params": [
    "Hay2tj3GcDYcE3AMHrej5WDsHGPVAYsegcubixLUvXUF",
    {
      "showInput": true,
      "showRawInput": false,
      "showEffects": true,
      "showEvents": true,
      "showObjectChanges": false,
      "showBalanceChanges": false,
      "showRawEffects": false
    }
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type                            | Required | Description                                       |
| --------- | ------------------------------- | -------- | ------------------------------------------------- |
| `digest`  | TransactionDigest               | Yes      | the digest of the queried transaction             |
| `options` | TransactionBlockResponseOptions | No       | options for specifying the content to be returned |

## Response

Returns `SuiTransactionBlockResponse`. (`TransactionBlockResponse`)

```json
{
  "digest": "Hay2tj3GcDYcE3AMHrej5WDsHGPVAYsegcubixLUvXUF",
  "transaction": {
    "data": {
      "messageVersion": "v1",
      "transaction": {
        "kind": "ProgrammableTransaction",
        "inputs": [
          {
            "type": "pure",
            "valueType": "address",
            "value": "0x8196d048b7a6d04c8edc89579d86fd3fc90c52f9a14c6b812b94fe613c5bcebb"
          },
          {
            "type": "object",
            "objectType": "immOrOwnedObject",
            "objectId": "0x5eeb1d449e2516166d57d71fdeb154d0dc9ecdb7b30057d0a932684cac352cdc",
            "version": "2",
            "digest": "GK4NxEKSrK88XkPNeuBqtJYPmU9yMTWMD7K9TdU4ybKN"
          }
        ],
        "transactions": [
          {
            "TransferObjects": [
              [
                {
                  "Input": 1
                }
              ],
              {
                "Input": 0
              }
            ]
          }
        ]
      },
      "sender": "0x82179c57d5895babfb655cd62e8e886a53334b5e7be9be658eb759cc35e3fc66",
      "gasData": {
        "payment": [
          {
            "objectId": "0x1a3e898029d024eec1d44c6af5e2facded84d03b5373514f16e3d66e00081051",
            "version": 2,
            "digest": "7nDZ5J4VyvYGUbX2f6mQdhkr3RFrb3vZqui1ogoyApD9"
          }
        ],
        "owner": "0x82179c57d5895babfb655cd62e8e886a53334b5e7be9be658eb759cc35e3fc66",
        "price": "10",
        "budget": "100000"
      }
    },
    "txSignatures": [
      "AMU7cJTEsJ5WoVlKZ2zsVuGMk9linDuNqLV9eGIIrqarP2x4R9riuvmmMgXfdxMm7jTzYxbHrsDNMwlxpTbbFghtCxWrqsEEHAdxoMDwblU5hyWJ8H3zFvk20E2fO5bzHA=="
    ]
  },
  "rawTransaction": "AQAAAAAAAgAggZbQSLem0EyO3IlXnYb9P8kMUvmh...0E2fO5bzHA==",
  "effects": {
    "messageVersion": "v1",
    "status": {
      "status": "success"
    },
    "executedEpoch": "0",
    "gasUsed": {
      "computationCost": "100",
      "storageCost": "100",
      "storageRebate": "10",
      "nonRefundableStorageFee": "0"
    },
    "transactionDigest": "6AyFnAuKAKCqm1cD94EyGzBqJCDDJ716ojjmsKF2rqoi",
    "mutated": [
      {
        "owner": {
          "AddressOwner": "0x82179c57d5895babfb655cd62e8e886a53334b5e7be9be658eb759cc35e3fc66"
        },
        "reference": {
          "objectId": "0x1a3e898029d024eec1d44c6af5e2facded84d03b5373514f16e3d66e00081051",
          "version": 2,
          "digest": "7nDZ5J4VyvYGUbX2f6mQdhkr3RFrb3vZqui1ogoyApD9"
        }
      },
      {
        "owner": {
          "AddressOwner": "0x8196d048b7a6d04c8edc89579d86fd3fc90c52f9a14c6b812b94fe613c5bcebb"
        },
        "reference": {
          "objectId": "0x5eeb1d449e2516166d57d71fdeb154d0dc9ecdb7b30057d0a932684cac352cdc",
          "version": 2,
          "digest": "GK4NxEKSrK88XkPNeuBqtJYPmU9yMTWMD7K9TdU4ybKN"
        }
      }
    ],
    "gasObject": {
      "owner": {
        "ObjectOwner": "0x82179c57d5895babfb655cd62e8e886a53334b5e7be9be658eb759cc35e3fc66"
      },
      "reference": {
        "objectId": "0x1a3e898029d024eec1d44c6af5e2facded84d03b5373514f16e3d66e00081051",
        "version": 2,
        "digest": "7nDZ5J4VyvYGUbX2f6mQdhkr3RFrb3vZqui1ogoyApD9"
      }
    },
    "eventsDigest": "9BQobwxQvJ1JxSXNn8v8htZPTu8FEzJJGgcD4kgLUuMd"
  },
  "objectChanges": [
    {
      "type": "transferred",
      "sender": "0x82179c57d5895babfb655cd62e8e886a53334b5e7be9be658eb759cc35e3fc66",
      "recipient": {
        "AddressOwner": "0x8196d048b7a6d04c8edc89579d86fd3fc90c52f9a14c6b812b94fe613c5bcebb"
      },
      "objectType": "0x2::example::Object",
      "objectId": "0x5eeb1d449e2516166d57d71fdeb154d0dc9ecdb7b30057d0a932684cac352cdc",
      "version": "2",
      "digest": "64UQ3a7m1mjWuzgyGoH8RnMyPGDN4XYTC9dS4qiSfdK4"
    }
  ]
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
