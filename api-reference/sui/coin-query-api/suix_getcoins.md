---
description: Return all Coin<`coin_type`> objects owned by an address.
---

# suix\_getCoins

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_getCoins",
  "params": [
    "0x6d907beaa3a49db57bdfdb3557e6d405cbf01c293a53e01457d65e92b5d8dd68",
    "0x2::sui::SUI",
    "0xee6b5173afedb35330f60397c2cbb48196ba41921246c304be7b490cee0904eb",
    3
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter   | Type       | Required | Description                                                                                                                                |
| ----------- | ---------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `owner`     | SuiAddress | Yes      | the owner's Sui address                                                                                                                    |
| `coin_type` | string     | No       | optional type name for the coin (e.g., 0x168da5bf1f48dafc111b0a488fa454aca95e0b5e::usdc::USDC), default to 0x2::sui::SUI if not specified. |
| `cursor`    | string     | No       | optional paging cursor                                                                                                                     |
| `limit`     | integer    | No       | maximum number of items per page                                                                                                           |

## Response

Returns `CoinPage`. (`Page_for_Coin_and_String`)

```json
{
  "data": [
    {
      "coinType": "0x2::sui::SUI",
      "coinObjectId": "0xd62ca040aba24f862a763851c54908cd2a0ee7d709c11b93d4a2083747b76856",
      "version": "103626",
      "digest": "C9fdokK19BpDCgUgWsJv3cfd4LDyk7WGYBeGhFHbEL2Z",
      "balance": "200000000",
      "previousTransaction": "tw5DzJTfdxTn4f3rekFrhN7dQTUezBgsEhycDobTBLb"
    },
    {
      "coinType": "0x2::sui::SUI",
      "coinObjectId": "0xf44d295a385dc3544d211411b865e8bc4f01f49186970c7cf61e1cc829cc0be7",
      "version": "103626",
      "digest": "5qZkmtN5J5uGHURtiy9BtBhnXATPR2Wa6BJBDLrMzCaf",
      "balance": "200000000",
      "previousTransaction": "AfgFe7ZfjJ5dWV6VAy2LbtvBFhcABkvdvwEjLrRcFqtr"
    },
    {
      "coinType": "0x2::sui::SUI",
      "coinObjectId": "0x42ef9314ccc792dd4401a88e69c66b4c5e43f21e9e57f4abe3c702649d3a7dd0",
      "version": "103626",
      "digest": "FLE2nB2Wio3oUyTx6HyzkrMsWiZxDg9Kk8s7ivvuoBbD",
      "balance": "200000000",
      "previousTransaction": "9er6jxigfuQEKsn9gtPV2oW1zGQRcFtKNijHVe88GUJD"
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
