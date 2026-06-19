---
description: >-
  This method provides the list of assets created by a particular creator. By
  using this method, developers and users can programmatically access and
  retrieve the assets created or owned by a specific c
---

# getAssetsByCreator

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": "123",
    "method": "getAssetsByCreator",
    "params": [
        "D3XrkNZz6wx6cofot7Zohsf2KSsu2ArngNk8VqU9cTY3",
        true,
        {
            "sortBy": "created",
            "sortDirection": "desc"
        },
        50,
        1,
        null,
        null
    ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Name           | Description                                                                                                                                                                                                               | Required |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| creatorAddress | The address of the creator of the assets.                                                                                                                                                                                 | Yes      |
| onlyVerified   | Indicates whether to retrieve only verified assets or not. Recommended to set to true to be sure that the asset belongs to the creator.                                                                                   | No       |
| sortBy         | Sorting criteria. This is specified as an object `{ sortBy: <value>, sortDirection: <value> }`, where `sortBy` is one of `["created", "updated", "recentAction", "none"]` and `sortDirection` is one of `["asc", "desc"]` | No       |
| page           | The index of the "page" to retrieve.                                                                                                                                                                                      | No       |
| before         | Fetch assets before the given ID.                                                                                                                                                                                         | No       |
| after          | Fetch assets after the given ID.                                                                                                                                                                                          | No       |
