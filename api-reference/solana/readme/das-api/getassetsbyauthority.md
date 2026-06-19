---
description: >-
  Fetch all the assets belonging to a specific authority. This allows paged
  responses in order to easily fetch a large number of records.
---

# getAssetsByAuthority

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": "123",
    "method": "getAssetsByAuthority",
    "params": [
        "3pMvTLUA9NzZQd4gi725p89mvND1wRNQM3C8XEv1hTdA",
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

| Name             | Description                                                                                                                                                                                       | Required |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| authorityAddress | Address of the asset authority                                                                                                                                                                    | Yes      |
| sortBy           | The Sorting Criteria: Defined as an object `{ sortBy: , sortDirection: }`, where `sortBy` can be `"created", "updated", "recentAction", or "none"`, and `sortDirection` can be `"asc" or "desc"`. | No       |
| limit            | The maximum number of assets to retrieve.                                                                                                                                                         | No       |
| page             | The index of the "page" to retrieve.                                                                                                                                                              | No       |
| before           | Fetch assets before the given ID.                                                                                                                                                                 | No       |
| after            | Fetch assets after the given ID.                                                                                                                                                                  | No       |
