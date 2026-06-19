---
description: >-
  The method returns a list of assets owned by a certain address or account. You
  can query the API for all assets associated with that owner by supplying the
  owner's pubkey as a parameter.
---

# getAssetsByOwner

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": "123",
    "method": "getAssetsByOwner",
    "params": [
        "9kPPbeBAvCtJCZ98EFKabxp7wTeFQRseCYDRdovyfUfz",
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

| Name         | Description                                                                                                                                                                                       | Required |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| ownerAddress | Address of the asset authority                                                                                                                                                                    | Yes      |
| sortBy       | The Sorting Criteria: Defined as an object `{ sortBy: , sortDirection: }`, where `sortBy` can be `"created", "updated", "recentAction", or "none"`, and `sortDirection` can be `"asc" or "desc"`. | Yes      |
| limit        | The maximum number of assets to retrieve.                                                                                                                                                         | No       |
| page         | The index of the "page" to retrieve.                                                                                                                                                              | No       |
| before       | Fetch assets before the given ID.                                                                                                                                                                 | No       |
| after        | Fetch assets after the given ID.                                                                                                                                                                  | No       |
