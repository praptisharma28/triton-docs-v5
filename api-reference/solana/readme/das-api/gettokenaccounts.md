---
description: >-
  Retrieve all token accounts linked to a specific mint or owner. Use this to
  efficiently list all holders of an SPL token or view all tokens owned by a
  given address.
---

# getTokenAccounts

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "id": "123",
    "jsonrpc": "2.0",
    "method": "getTokenAccounts",
    "params": {
      "mintAddress": "BUaiggSfm81ZRAeW572dNf1BhXCzZxaWrGNRm5PjSQzY",
      "ownerAddress": "BiKcVb6t6YDZczYjWLjXKwdgAUecdQrp1EJXAfRmu3VR",
      "page": 1,
      "limit": 100
    }
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Name           | Description                       | Required            |
| -------------- | --------------------------------- | ------------------- |
| `mintAddress`  | The address of associated mint.   | Yes or ownerAddress |
| `ownerAddress` | The owner address of the tokens.  | Yes or mintAddress  |
| `page`         | The current pagination page.      | No                  |
| `limit`        | Number of results per page.       | No                  |
| `cursor`       | Optional pagination cursor.       | No                  |
| `before`       | Return results before the cursor. | No                  |
| `after`        | Return results after the cursor.  | No                  |
