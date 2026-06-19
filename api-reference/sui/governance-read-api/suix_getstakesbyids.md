---
description: >-
  Return one or more [DelegatedStake]. If a Stake was withdrawn its status will
  be Unstaked.
---

# suix\_getStakesByIds

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_getStakesByIds",
  "params": [
    [
      "0x378423de90ed03b694cecf443c72b5387b29a731d26d98108d7abc4902107d7d",
      "0x6a8e0f8fea6fda5488462e58724c034462b6064a08845e2ae2942fe7c4ee816d"
    ]
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter        | Type  | Required | Description |
| ---------------- | ----- | -------- | ----------- |
| `staked_sui_ids` | array | Yes      |             |

## Response

Returns `Vec<DelegatedStake>`. (`array`)

```json
{
  "validatorAddress": "0x754eb2eed23e6c6bb32c89fe1f21ab588374445e72e0402aea014b2956105799",
  "stakingPool": "0x63ee67e81398729f87d81d62f399c041b0f8d0938923ea7e3917608ee62df437",
  "stakes": [
    {
      "stakedSuiId": "0x378423de90ed03b694cecf443c72b5387b29a731d26d98108d7abc4902107d7d",
      "stakeRequestEpoch": "62",
      "stakeActiveEpoch": "63",
      "principal": "200000000000",
      "status": "Active",
      "estimatedReward": "520000000"
    },
    {
      "stakedSuiId": "0x6a8e0f8fea6fda5488462e58724c034462b6064a08845e2ae2942fe7c4ee816d",
      "stakeRequestEpoch": "244",
      "stakeActiveEpoch": "245",
      "principal": "200000000000",
      "status": "Unstaked"
    }
  ]
}
```
