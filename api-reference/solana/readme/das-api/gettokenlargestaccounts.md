---
description: Retrieve the 20 largest token accounts for a specific SPL Token.
---

# getTokenLargestAccounts

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": "123",
    "method": "getTokenLargestAccounts",
     "params": [
       "jovWMTogqDpoEWUSFSns2Y9rkJkfJHsTgPWJgzCjwCF",
       {
         "commitment": "confirmed"
       }
     ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Name   | Description                                     |
| ------ | ----------------------------------------------- |
| pubkey | The unique identifier of the token to retrieve. |
