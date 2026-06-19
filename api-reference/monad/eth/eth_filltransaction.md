---
description: >-
  Fill in the defaults (gas, nonce, fees) for a transaction call object and
  return the unsigned transaction.
---

# eth\_fillTransaction

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_fillTransaction",
  "params": [
    {
      "to": "0x...",
      "data": "0x..."
    }
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type   | Required | Description              |
| --------- | ------ | -------- | ------------------------ |
| `tx`      | object | Yes      | Transaction call object. |

## Response

```json
{
  "raw": "0x",
  "tx": {
    "from": null,
    "to": null,
    "gas": null,
    "maxFeePerGas": null,
    "maxPriorityFeePerGas": null,
    "value": null,
    "input": null,
    "data": null,
    "nonce": null,
    "chainId": null,
    "accessList": null,
    "authorizationList": null
  }
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
