---
description: >-
  This method retrieves the transaction signatures associated with a compressed
  digital asset/NFT. It can be identified by its ID or by tree and leaf index.
---

# getSignaturesForAsset

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": "123",
    "method": "getAssetSignatures",
    "params": {
      "id": "EZsmgudhX6EFVfrdxmShDiNj365E85x7kpLJCVvPLVSH"
    }
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Name      | Description                                    | Required       |
| --------- | ---------------------------------------------- | -------------- |
| id        | The unique identifier of the compressed asset. | or tree + leaf |
| tree      | The tree corresponding to the leaf.            | or id          |
| leafIndex | The leaf index of digital compressed asset.    | or id          |
