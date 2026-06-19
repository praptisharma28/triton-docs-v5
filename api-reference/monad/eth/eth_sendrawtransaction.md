---
description: >-
  Submits a raw transaction. You can create and sign a transaction externally
  using a library such as [web3.js](https://web3js.readthedocs.io/) or
  [ethers.js](https://docs.ethers.org/). For [EIP-4844](h
---

# eth\_sendRawTransaction

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-monad-endpoint>.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_sendRawTransaction",
  "params": [
    "0xf869018203e882520894f17f52151ebef6c7334fad080c5704d77216b732881bc16d674ec80000801ba02da1c48b670996dcb1f447ef9ef00b33033c48a4fe938f420bec3e56bfd24071a062e0aa78a81bf0290afbc3a9d8e9a068e6d74caa66c5e0fa8a46deaae96b0833"
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter     | Type  | Required | Description |
| ------------- | ----- | -------- | ----------- |
| `Transaction` | bytes | Yes      |             |

## Response

Returns `Transaction hash`.

```json
"0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
```
