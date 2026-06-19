---
description: >-
  Add, list, and delete address watch lists on a subscription (operator and
  reseller tokens only).
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: false
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# Address watch lists

{% hint style="info" %}
This feature is limited to account management API tokens created with the operator or reseller role.
{% endhint %}

## Add watch list

<mark style="color:blue;">`POST /api/v1/subscriptions/:subscription-uuid/address_watch_lists`</mark>

**Parameters**

<table><thead><tr><th width="263.3333333333333">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>variant</code> <mark style="color:red;">*required</mark></td><td><code>string</code></td><td>Must be either <code>collection</code> or <code>tree</code></td></tr><tr><td><code>encoded_value</code> <mark style="color:red;">*required</mark></td><td><code>string</code></td><td></td></tr></tbody></table>

## List watch lists

<mark style="color:blue;">`GET /api/v1/address_watch_lists`</mark>

**Query parameters**

| Name                | Type     | Description |
| ------------------- | -------- | ----------- |
| `variant`           | `string` |             |
| `subscription_uuid` | `string` |             |
| `subscription_type` | `string` |             |

## Delete watch list

<mark style="color:blue;">`DELETE /api/v1/address_watch_lists/:uuid`</mark>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
