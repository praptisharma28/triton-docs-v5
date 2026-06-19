---
description: >-
  Fetches the proof path for a given compressed asset/NFT. This method is
  required for submitting changes to a compressed merkle tree which requires the
  proof path for the tree it belongs to.
---

# getAssetProof

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": "123",
  "method": "getAssetProof",
  "params": {
    "id": "EZsmgudhX6EFVfrdxmShDiNj365E85x7kpLJCVvPLVSH"
  }
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Name | Description                                    | Required |
| ---- | ---------------------------------------------- | -------- |
| id   | The unique identifier of the compressed asset. | Yes      |
