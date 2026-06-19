---
description: Return the total number of transaction blocks known to the server.
---

# sui\_getTotalTransactionBlocks

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_getTotalTransactionBlocks",
  "params": []
}'
```
{% endtab %}
{% endtabs %}

## Response

Returns `BigInt<u64>`. (`BigInt_for_uint64`)

```json
"2451485"
```
