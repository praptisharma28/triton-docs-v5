---
description: >-
  Return the protocol config table for the given version number. If none is
  specified, the node uses the version of the latest epoch it has processed.
---

# sui\_getProtocolConfig

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sui_getProtocolConfig",
  "params": [
    6
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter | Type                | Required | Description                                                                                                         |
| --------- | ------------------- | -------- | ------------------------------------------------------------------------------------------------------------------- |
| `version` | BigInt\_for\_uint64 | No       | An optional protocol version specifier. If omitted, the latest protocol config table for the node will be returned. |

## Response

Returns `ProtocolConfigResponse`. (`ProtocolConfig`)

The full `ProtocolConfigResponse` response is large; call the method to see every field.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
