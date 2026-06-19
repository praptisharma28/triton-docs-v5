---
description: Returns the information about a transaction requested by transaction hash.
---

# eth\_getTransactionByHash

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_getTransactionByHash",
  "params": [
    "0xa52be92809541220ee0aaaede6047d9a6c5d0cd96a517c854d944ee70a0ebb44"
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

Returns `Transaction information`.

```json
{
  "blockHash": "0x510efccf44a192e6e34bcb439a1947e24b86244280762cbb006858c237093fda",
  "blockNumber": "0x422",
  "chainId": "0x7e2",
  "from": "0xfe3b557e8fb62b89f4916b721be55ceb828dbd73",
  "gas": "0x5208",
  "gasPrice": "0x3b9aca00",
  "hash": "0xa52be92809541220ee0aaaede6047d9a6c5d0cd96a517c854d944ee70a0ebb44",
  "input": "0x",
  "nonce": "0x1",
  "to": "0x627306090abab3a6e1400e9345bc60c78a8bef57",
  "transactionIndex": "0x0",
  "value": "0x4e1003b28d9280000",
  "v": "0xfe7",
  "r": "0x84caf09aefbd5e539295acc67217563438a4efb224879b6855f56857fa2037d3",
  "s": "0x5e863be3829812c81439f0ae9d8ecb832b531d651fb234c848d1bf45e62be8b9"
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
