---
description: >-
  Runs the transaction in dev-inspect mode. Which allows for nearly any
  transaction (or Move call) with any arguments. Detailed results are provided,
  including both the transaction effects and any retur.
---

# sui\_devInspectTransactionBlock

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_devInspectTransactionBlock",
  "params": [
    "0xd70420418b84502e506794227f897237764dde8d79a01ab2104bf742a277a2ab",
    "AAACACBnxtMcbJcOVn8D72fYEaT4Q2ZbjePygvpI...oIYBAAAAAAAA",
    1000,
    8888,
    null
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter         | Type                | Required | Description                                                                                      |
| ----------------- | ------------------- | -------- | ------------------------------------------------------------------------------------------------ |
| `sender_address`  | SuiAddress          | Yes      |                                                                                                  |
| `tx_bytes`        | Base64              | Yes      | BCS encoded TransactionKind(as opposed to TransactionData, which include gasBudget and gasPrice) |
| `gas_price`       | BigInt\_for\_uint64 | No       | Gas is not charged, but gas usage is still calculated. Default to use reference gas price        |
| `epoch`           | BigInt\_for\_uint64 | No       | The epoch to perform the call. Will be set from the system state object if not provided          |
| `additional_args` | DevInspectArgs      | No       | Additional arguments including gas\_budget, gas\_objects, gas\_sponsor and skip\_checks.         |

## Response

Returns `DevInspectResults`.

```json
{
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
    "transactionDigest": "76gyHCk7FRrGACRqXM7Ybj5uJLtAzgEMJ5P9CeEzxZSG",
    "mutated": [
      {
        "owner": {
          "AddressOwner": "0x2f5c543d716ae43d453deb3b199bbdaf1bc7030c7682f87703dce431248fede0"
        },
        "reference": {
          "objectId": "0x2c43805d0d6f5402a7ad0da5bdec549c7cdea06bc4a1d339a22521eea933b889",
          "version": 2,
          "digest": "BhbWpBeESxuRWvmvLMyb2JNUuFa6j4aG1T4WUiPgKAHm"
        }
      },
      {
        "owner": {
          "AddressOwner": "0x67c6d31c6c970e567f03ef67d811a4f843665b8de3f282fa48b3e0103ba9bbed"
        },
        "reference": {
          "objectId": "0x6a06153b9fc486e101f0e9e2703ac8666d06aecc4ddf7e7926a361970c65a5b1",
          "version": 2,
          "digest": "GdfET1avZReDftpJNB8LSuHJ2cKUheSbEaLMzuPVXHsM"
        }
      }
    ],
    "gasObject": {
      "owner": {
        "ObjectOwner": "0x2f5c543d716ae43d453deb3b199bbdaf1bc7030c7682f87703dce431248fede0"
      },
      "reference": {
        "objectId": "0x2c43805d0d6f5402a7ad0da5bdec549c7cdea06bc4a1d339a22521eea933b889",
        "version": 2,
        "digest": "BhbWpBeESxuRWvmvLMyb2JNUuFa6j4aG1T4WUiPgKAHm"
      }
    },
    "eventsDigest": "6kerMphN4S5QTfd9TAhwMiFq1q9c2YwfpheBfWm85vUq"
  },
  "events": []
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
