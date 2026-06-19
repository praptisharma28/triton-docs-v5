---
description: Return the committee information for the asked `epoch`.
---

# suix\_getCommitteeInfo

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_getCommitteeInfo",
  "params": [
    "5000"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type                | Required | Description                                                 |
| --------- | ------------------- | -------- | ----------------------------------------------------------- |
| `epoch`   | BigInt\_for\_uint64 | No       | The epoch of interest. If None, default to the latest epoch |

## Response

Returns `SuiCommittee`. (`CommitteeInfo`)

```json
{
  "epoch": "5000",
  "validators": [
    [
      "jc/20VUECmVvSBmxMRG1LFdGqGunLzlfuv4uw4R9HoFA5iSnUf32tfIFC8cgXPnTAATJCwx0Cv/TJs5nPMKyOi0k1T4q/rKG38Zo/UBgCJ1tKxe3md02+Q0zLlSnozjU",
      "2500"
    ],
    [
      "mfJe9h+AMrkUY2RgmCxcxvE07x3a52ZX8sv+wev8jQlzdAgN9vzw3Li8Sw2OCvXYDrv/K0xZn1T0LWMS38MUJ2B4wcw0fru+xRmL4lhRPzhrkw0CwnSagD4jMJVevRoQ",
      "2500"
    ],
    [
      "rd7vlNiYyI5A297/kcXxBfnPLHR/tvK8N+wD1ske2y4aV4z1RL6LCTHiXyQ9WbDDDZihbOO6HWzx1/UEJpkusK2zE0sFW+gUDS218l+wDYP45CIr8B/WrJOh/0152ljy",
      "2500"
    ],
    [
      "s/1e+1yHJAOkrRPxGZUTYG0jNUqEUkmuoVdWTCP/PBXGyeZSty10DoysuTy8wGhrDsDMDBx2C/tCtDZRn8WoBUt2UzqXqfI5h9CX75ax8lJrsgc/oQp3GZQXcjR+8nT0",
      "2500"
    ]
  ]
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
