---
description: Returns the receipt of a transaction by transaction hash.
---

# eth\_getTransactionReceipt

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_getTransactionReceipt",
  "params": [
    "0x504ce587a65bdbdb6414a0c6c16d86a04dd79bfcc4f2950eec9634b30ce5370f"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter          | Type   | Required | Description |
| ------------------ | ------ | -------- | ----------- |
| `Transaction hash` | hash32 | Yes      |             |

## Response

Returns `Receipt information`.

```json
{
  "blockHash": "0xe7212a92cfb9b06addc80dec2a0dfae9ea94fd344efeb157c41e12994fcad60a",
  "blockNumber": "0x50",
  "contractAddress": null,
  "cumulativeGasUsed": "0x5208",
  "from": "0x627306090abab3a6e1400e9345bc60c78a8bef57",
  "gasUsed": "0x5208",
  "blobGasUsed": "0x20000",
  "effectiveGasPrice": "0x1",
  "blobGasPrice": "0x3",
  "logs": [],
  "logsBloom": "0x00000000000000000000000000000000000000...000000000000",
  "status": "0x1",
  "to": "0xf17f52151ebef6c7334fad080c5704d77216b732",
  "transactionHash": "0xc00e97af59c6f88de163306935f7682af1a34c67245e414537d02e422815efc3",
  "transactionIndex": "0x0"
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
