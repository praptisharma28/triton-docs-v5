---
description: >-
  Execute the transaction and wait for results if desired. Request types: 1.
  WaitForEffectsCert: waits for TransactionEffectsCert and then return to
  client. This mode is a proxy for transaction finality
---

# sui\_executeTransactionBlock

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_executeTransactionBlock",
  "params": [
    "AAACACBqEB6aOvXIBwES+Ahkizbvv43uihqC3kbZ...oIYBAAAAAAAA",
    [
      "AKD4XdltkCyBi1Heb4EJJ3lzuV3F4u7+CYeaE+Fd7qXpaT17yd4tHWjMf4CWq3TuXBLxTpkc2MV39P6p7eMV8QnqvbuA0Q1Bqu4RHV3JPpqmH+C527hWJGUBOZN1j9sg8w=="
    ],
    {
      "showInput": true,
      "showRawInput": true,
      "showEffects": true,
      "showEvents": true,
      "showObjectChanges": true,
      "showBalanceChanges": true,
      "showRawEffects": false
    },
    "WaitForLocalExecution"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter      | Type                            | Required | Description                                                                                                                                                                                 |
| -------------- | ------------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `tx_bytes`     | Base64                          | Yes      | BCS serialized transaction data bytes without its type tag, as base-64 encoded string.                                                                                                      |
| `signatures`   | array                           | Yes      | A list of signatures (`flag \|\| signature \|\| pubkey` bytes, as base-64 encoded string). Signature is committed to the intent message of the transaction data, as base-64 encoded string. |
| `options`      | TransactionBlockResponseOptions | No       | options for specifying the content to be returned                                                                                                                                           |
| `request_type` | ExecuteTransactionRequestType   | No       | The request type, derived from `SuiTransactionBlockResponseOptions` if None                                                                                                                 |

## Response

Returns `SuiTransactionBlockResponse`. (`TransactionBlockResponse`)

```json
{
  "digest": "Gx7V6EteEAqSsptc1kbi1nUEMP7mhr4qV2cYS7JjzbBd",
  "transaction": {
    "data": {
      "messageVersion": "v1",
      "transaction": {
        "kind": "ProgrammableTransaction",
        "inputs": [
          {
            "type": "pure",
            "valueType": "address",
            "value": "0x6a101e9a3af5c8070112f808648b36efbf8dee8a1a82de46d9504e96a1108a17"
          },
          {
            "type": "object",
            "objectType": "immOrOwnedObject",
            "objectId": "0x8ef76f56c399633a2eb310bca9124e5f2f38ce739eaacbb6600688804e078448",
            "version": "2",
            "digest": "9Tvs1pGrMbNv7kkr1PoKLsWamyQpaFz5UWbL2AQ1ezk2"
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
      "sender": "0x04b497a916b4656609a7fa827a09184d6f47681d3ba74ce0d7a36c64b2b1f887",
      "gasData": {
        "payment": [
          {
            "objectId": "0x9f90f7f866cc29dfb1da6373ce94f9d95dca5a40f4e1769918eebf9fbae00bd3",
            "version": 2,
            "digest": "AaeJbTYkUuyromsivxzkoxSkHt7pCESTyQG7xz6nbQ2G"
          }
        ],
        "owner": "0x04b497a916b4656609a7fa827a09184d6f47681d3ba74ce0d7a36c64b2b1f887",
        "price": "10",
        "budget": "100000"
      }
    },
    "txSignatures": [
      "AKD4XdltkCyBi1Heb4EJJ3lzuV3F4u7+CYeaE+Fd7qXpaT17yd4tHWjMf4CWq3TuXBLxTpkc2MV39P6p7eMV8QnqvbuA0Q1Bqu4RHV3JPpqmH+C527hWJGUBOZN1j9sg8w=="
    ]
  },
  "rawTransaction": "AQAAAAAAAgAgahAemjr1yAcBEvgIZIs277+N7ooa...OZN1j9sg8w==",
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
    "transactionDigest": "9agZ3azEMgMqxrDVG8P4GddELfWag2HhimEkpjixHhGE",
    "mutated": [
      {
        "owner": {
          "AddressOwner": "0x04b497a916b4656609a7fa827a09184d6f47681d3ba74ce0d7a36c64b2b1f887"
        },
        "reference": {
          "objectId": "0x9f90f7f866cc29dfb1da6373ce94f9d95dca5a40f4e1769918eebf9fbae00bd3",
          "version": 2,
          "digest": "AaeJbTYkUuyromsivxzkoxSkHt7pCESTyQG7xz6nbQ2G"
        }
      },
      {
        "owner": {
          "AddressOwner": "0x6a101e9a3af5c8070112f808648b36efbf8dee8a1a82de46d9504e96a1108a17"
        },
        "reference": {
          "objectId": "0x8ef76f56c399633a2eb310bca9124e5f2f38ce739eaacbb6600688804e078448",
          "version": 2,
          "digest": "9Tvs1pGrMbNv7kkr1PoKLsWamyQpaFz5UWbL2AQ1ezk2"
        }
      }
    ],
    "gasObject": {
      "owner": {
        "ObjectOwner": "0x04b497a916b4656609a7fa827a09184d6f47681d3ba74ce0d7a36c64b2b1f887"
      },
      "reference": {
        "objectId": "0x9f90f7f866cc29dfb1da6373ce94f9d95dca5a40f4e1769918eebf9fbae00bd3",
        "version": 2,
        "digest": "AaeJbTYkUuyromsivxzkoxSkHt7pCESTyQG7xz6nbQ2G"
      }
    },
    "eventsDigest": "816hEv4WAW2reK9xkf11PeHiaZJrp7PQT9oGJZhdf9TN"
  },
  "objectChanges": [
    {
      "type": "transferred",
      "sender": "0x04b497a916b4656609a7fa827a09184d6f47681d3ba74ce0d7a36c64b2b1f887",
      "recipient": {
        "AddressOwner": "0x6a101e9a3af5c8070112f808648b36efbf8dee8a1a82de46d9504e96a1108a17"
      },
      "objectType": "0x2::example::Object",
      "objectId": "0x8ef76f56c399633a2eb310bca9124e5f2f38ce739eaacbb6600688804e078448",
      "version": "2",
      "digest": "7PsBHpUW6yfGNov2WrbVafLjgT9nYziQ3gVDbRq6zTbF"
    }
  ]
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
