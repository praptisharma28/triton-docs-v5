---
description: Return all [DelegatedStake].
---

# suix\_getStakes

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_getStakes",
  "params": [
    "0x9c76d5157eaa77c41a7bfda8db98a8e8080f7cb53b7313088ed085c73f866f21"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type       | Required | Description |
| --------- | ---------- | -------- | ----------- |
| `owner`   | SuiAddress | Yes      |             |

## Response

Returns `Vec<DelegatedStake>`. (`array`)

```json
[
  {
    "validatorAddress": "0x3befb84f03a24386492bd3b05b1fd386172eb450e5059ce7df0ea6d9d6cefcaa",
    "stakingPool": "0x9a95cf69368e31b4dbe8ee9bdb3c0587bbc79d8fc6edf4007e185a962fd906df",
    "stakes": [
      {
        "stakedSuiId": "0xb4eeb46b70f0bebcae832aeef9f7c5db76052ab656e5f81853d0cf701cdbc8eb",
        "stakeRequestEpoch": "62",
        "stakeActiveEpoch": "63",
        "principal": "200000000000",
        "status": "Active",
        "estimatedReward": "520000000"
      },
      {
        "stakedSuiId": "0xf27ab513fc6ef8c344406c78da3d5ad3a5fcc295dc8803c15989a62d33ee8590",
        "stakeRequestEpoch": "142",
        "stakeActiveEpoch": "143",
        "principal": "200000000000",
        "status": "Pending"
      }
    ]
  },
  {
    "validatorAddress": "0x14cfd5e91c13a481370240e392464c329a203fb9f0a8158aaab9b2a90044b26e",
    "stakingPool": "0x14cc7fee4100fdcabda6d15c63c4b49c45ae23f2b936495cd38b1a4b04010295",
    "stakes": [
      {
        "stakedSuiId": "0xbaa75ac72e548aeecf2ce8b4e88530651d6e8f93e0fb79b4bc65a512beb5b9f3",
        "stakeRequestEpoch": "244",
        "stakeActiveEpoch": "245",
        "principal": "200000000000",
        "status": "Unstaked"
      }
    ]
  }
]
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
