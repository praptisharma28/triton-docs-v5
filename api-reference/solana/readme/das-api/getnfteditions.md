---
description: Get all printable editions for a master edition NFT mint.
---

# getNftEditions

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "id": "123",
    "jsonrpc": "2.0",
    "method": "getNftEditions",
    "params": {
      "mintAddress": "BUaiggSfm81ZRAeW572dNf1BhXCzZxaWrGNRm5PjSQzY",
      "page": 1,
      "limit": 100
    }
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Name          | Description                             | Required |
| ------------- | --------------------------------------- | -------- |
| `mintAddress` | The mint address of the master edition. | Yes      |
| `page`        | The current pagination page.            | No       |
| `limit`       | Number of results per page.             | No       |
| `cursor`      | Optional pagination cursor.             | No       |
| `before`      | Return results before the cursor.       | No       |
| `after`       | Return results after the cursor.        | No       |
