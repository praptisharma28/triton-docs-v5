---
description: Return the sequence number of the latest checkpoint that has been executed
---

# sui\_getLatestCheckpointSequenceNumber

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_getLatestCheckpointSequenceNumber",
  "params": []
}'
```
{% endtab %}
{% endtabs %}

## Response

Returns `BigInt<u64>`. (`BigInt_for_uint64`)

```json
"507021"
```
