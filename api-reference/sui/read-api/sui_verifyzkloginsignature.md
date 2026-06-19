---
description: Verify a zklogin signature for the given bytes, intent scope and author.
---

# sui\_verifyZkLoginSignature

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_verifyZkLoginSignature",
  "params": [
    "<bytes>",
    "<signature>",
    "<intent_scope>",
    "<author>"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter      | Type               | Required | Description                                                                                             |
| -------------- | ------------------ | -------- | ------------------------------------------------------------------------------------------------------- |
| `bytes`        | string             | Yes      | The Base64 string of bcs bytes for raw transaction data or personal message indicated by intent\_scope. |
| `signature`    | string             | Yes      | The Base64 string of the zklogin signature to verify.                                                   |
| `intent_scope` | ZkLoginIntentScope | Yes      | The intent scope, either transaction data or personal message. Used to parse bytes.                     |
| `author`       | SuiAddress         | Yes      | The author of the signature.                                                                            |

## Response

Returns `ZkLoginVerifyResult`.
