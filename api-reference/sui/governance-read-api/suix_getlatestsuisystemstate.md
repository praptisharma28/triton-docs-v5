---
description: Return the latest SUI system state object on-chain.
---

# suix\_getLatestSuiSystemState

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_getLatestSuiSystemState",
  "params": []
}'
```
{% endtab %}
{% endtabs %}

## Response

Returns `SuiSystemStateSummary`.

```json
"some_system_state"
```
