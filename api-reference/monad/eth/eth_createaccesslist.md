---
description: Generates an access list for a transaction.
---

# eth\_createAccessList

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_createAccessList",
  "params": [
    {
      "from": "0xaea8f8f781326bfe6a7683c2bd48dd6aa4d3ba63",
      "data": "0x608060806080608155"
    },
    "latest"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter     | Type               | Required | Description |
| ------------- | ------------------ | -------- | ----------- |
| `Transaction` | GenericTransaction | Yes      |             |
| `Block`       | BlockNumberOrTag   | No       |             |

## Response

Returns `Gas used`.

```json
{
  "accessList": [
    {
      "address": "0xa02457e5dfd32bda5fc7e1f1b008aa5979568150",
      "storageKeys": [
        "0x0000000000000000000000000000000000000000000000000000000000000081"
      ]
    }
  ],
  "gasUsed": "0x125f8"
}
```
