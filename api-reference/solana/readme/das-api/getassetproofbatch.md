---
description: Get merkle proofs for compressed assets by their IDs.
---

# getAssetProofBatch

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": "123",
  "method": "getAssetProofs",
  "params": {
    "ids": [
      "EZsmgudhX6EFVfrdxmShDiNj365E85x7kpLJCVvPLVSH",
      "Hhxb7pNvwi76WTSqFjMX5yXjSso1S3agEJ9kQZP9NQeJ"
    ]
  }
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Name | Description                                 | Required |
| ---- | ------------------------------------------- | -------- |
| ids  | A set of asset IDs used to retrieve proofs. | Yes      |
