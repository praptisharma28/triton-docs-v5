---
description: Returns the receipts of a block by number or hash.
---

# eth\_getBlockReceipts

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_getBlockReceipts",
  "params": [
    "latest"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type                   | Required | Description |
| --------- | ---------------------- | -------- | ----------- |
| `Block`   | BlockNumberOrTagOrHash | Yes      |             |

## Response

Returns `Receipts information`.

```json
[
  {
    "blockHash": "0x19514ce955c65e4dd2cd41f435a75a46a08535b8fc16bc660f8092b32590b182",
    "blockNumber": "0x6f55",
    "contractAddress": null,
    "cumulativeGasUsed": "0x18c36",
    "from": "0x22896bfc68814bfd855b1a167255ee497006e730",
    "gasUsed": "0x18c36",
    "blobGasUsed": "0x20000",
    "effectiveGasPrice": "0x9502f907",
    "blobGasPrice": "0x32",
    "logs": [
      {
        "address": "0xfd584430cafa2f451b4e2ebcf3986a21fff04350",
        "topics": [
          "0x2f8788117e7eff1d82e926ec794901d17c78024a50270940304540a733656f0d",
          "0x4be29e0e4eb91f98f709d98803cba271592782e293b84a625e025cbb40197ba8",
          "0x000000000000000000000000835281a2563db4ebf1b626172e085dc406bfc7d2",
          "0x00000000000000000000000022896bfc68814bfd855b1a167255ee497006e730"
        ],
        "data": "0x",
        "blockNumber": "0x6f55",
        "transactionHash": "0x4a481e4649da999d92db0585c36cba94c18a33747e95dc235330e6c737c6f975",
        "transactionIndex": "0x0",
        "blockHash": "0x19514ce955c65e4dd2cd41f435a75a46a08535b8fc16bc660f8092b32590b182",
        "logIndex": "0x0",
        "removed": false
      }
    ],
    "logsBloom": "0x00000004000000000000000000000000000000...000000000000",
    "status": "0x1",
    "to": "0xfd584430cafa2f451b4e2ebcf3986a21fff04350",
    "transactionHash": "0x4a481e4649da999d92db0585c36cba94c18a33747e95dc235330e6c737c6f975",
    "transactionIndex": "0x0",
    "type": "0x0"
  },
  {
    "blockHash": "0x19514ce955c65e4dd2cd41f435a75a46a08535b8fc16bc660f8092b32590b182",
    "blockNumber": "0x6f55",
    "contractAddress": null,
    "cumulativeGasUsed": "0x1de3e",
    "from": "0x712e3a792c974b3e3dbe41229ad4290791c75a82",
    "gasUsed": "0x5208",
    "blobGasUsed": "0x20000",
    "effectiveGasPrice": "0x9502f907",
    "blobGasPrice": "0x32",
    "logs": [],
    "logsBloom": "0x00000000000000000000000000000000000000...000000000000",
    "status": "0x1",
    "to": "0xd42e2b1c14d02f1df5369a9827cb8e6f3f75f338",
    "transactionHash": "0xefb83b4e3f1c317e8da0f8e2fbb2fe964f34ee184466032aeecac79f20eacaf6",
    "transactionIndex": "0x1",
    "type": "0x2"
  }
]
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
