---
description: Return the total coin balance for all coin type, owned by the address owner.
---

# suix\_getAllBalances

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_getAllBalances",
  "params": [
    "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type       | Required | Description             |
| --------- | ---------- | -------- | ----------------------- |
| `owner`   | SuiAddress | Yes      | the owner's Sui address |

## Response

Returns `Vec<Balance>`. (`array`)

```json
[
  {
    "coinType": "0x2::sui::SUI",
    "coinObjectCount": 15,
    "totalBalance": "3000000000",
    "lockedBalance": {},
    "fundsInAddressBalance": "42"
  }
]
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
