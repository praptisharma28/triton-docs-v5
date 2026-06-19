---
description: Return all Coin objects owned by an address.
---

# suix\_getAllCoins

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_getAllCoins",
  "params": [
    "0x41f5975e3c6bd5c95f041a8493ad7e9934be26e69152d2c2e86d8a9bdbd242b3",
    "0x2564cd31a71cf9833609b111436d8f0f47b7f8b9927ec3f8975a1dcbf9b25564",
    3
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type       | Required | Description                      |
| --------- | ---------- | -------- | -------------------------------- |
| `owner`   | SuiAddress | Yes      | the owner's Sui address          |
| `cursor`  | string     | No       | optional paging cursor           |
| `limit`   | integer    | No       | maximum number of items per page |

## Response

Returns `CoinPage`. (`Page_for_Coin_and_String`)

```json
{
  "data": [
    {
      "coinType": "0x2::sui::SUI",
      "coinObjectId": "0x861c5e055605b2bb1199faf653a8771e448930bc95a0369fad43a9870a2e5878",
      "version": "103626",
      "digest": "Ao1QyN9UTmYzb2ead3D5xhSBk7TvACRvmnJW8gRbwP99",
      "balance": "200000000",
      "previousTransaction": "7dp5WtTmtGp83EXYYFMzjBJRFeSgR67AzqMETLrfgeFx"
    },
    {
      "coinType": "0x2::sui::SUI",
      "coinObjectId": "0x7e769678d059761bff8a8f3944642e4c33a6e4fb0b55f8face36fadaa22f2a0d",
      "version": "103626",
      "digest": "5taVxHU9QLQD5cNdqxt8kNGAab93GMG4vX7zYDxEaohx",
      "balance": "200000000",
      "previousTransaction": "9xLdMXezY8d1yRA2TtN6pYjapyy2EVKHWNriGPFGCFvd"
    },
    {
      "coinType": "0x2::sui::SUI",
      "coinObjectId": "0xa323d541ba5cf9e34919d2644cda38a263f69f47ae954dec65295231e0d2c7c8",
      "version": "103626",
      "digest": "82ZNKSSueWUQkpFNbBZGHSr3sUL5Rxfr7ucVRsvgQzz2",
      "balance": "200000000",
      "previousTransaction": "5xexWFq6QpGHBQyC9P2cbAJXq9qm2EjzfuRM9NwS1uyG"
    }
  ],
  "nextCursor": "abcd",
  "hasNextPage": true
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
