---
description: >-
  Submit a signed raw transaction and wait for its receipt, returning the
  receipt synchronously.
---

# eth\_sendRawTransactionSync

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_sendRawTransactionSync",
  "params": [
    "0x...",
    "0x..."
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter    | Type    | Required | Description                                        |
| ------------ | ------- | -------- | -------------------------------------------------- |
| `hex_tx`     | string  | Yes      | Signed transaction data (hex-encoded).             |
| `timeout_ms` | integer | Yes      | How long to wait for the receipt, in milliseconds. |

## Response

```json
{
  "type": "0x2",
  "status": "0x1",
  "cumulativeGasUsed": "0xa42aec",
  "logs": [
    {
      "address": "0xdac17f958d2ee523a2206206994597c13d831ec7",
      "topics": [
        "0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925"
      ],
      "data": "0x00000000000000000000000000000000000000000052b7d2dcc80cd2e4000000",
      "blockHash": "0x4acbdefb861ef4adedb135ca52865f6743451bfbfa35db78076f881a40401a5e",
      "blockNumber": "0x129f4b9",
      "transactionHash": "0x21f6554c28453a01e7276c1db2fc1695bb512b170818bfa98fa8136433100616",
      "transactionIndex": "0x7f",
      "logIndex": "0x118",
      "removed": false
    }
  ],
  "logsBloom": "0x00000000...",
  "transactionHash": "0x21f6554c28453a01e7276c1db2fc1695bb512b170818bfa98fa8136433100616",
  "transactionIndex": "0x7f",
  "blockHash": "0x4acbdefb861ef4adedb135ca52865f6743451bfbfa35db78076f881a40401a5e",
  "blockNumber": "0x129f4b9",
  "gasUsed": "0xbde1",
  "effectiveGasPrice": "0xfb0f6e8c9",
  "from": "0x9a53bfba35269414f3b2d20b52ca01b15932c7b2",
  "to": "0xdac17f958d2ee523a2206206994597c13d831ec7",
  "contractAddress": null
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
