---
description: List, create, and update subscription types.
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
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# Subscription types

## **Overview**

A Subscription Type defines what kind of pool a subscription is for. This is how endpoints are distinguished between a dedicated pool and a shared pool. It also can determine which Solana RPC environment the subscription and endpoints target.

The developer subscription is used to encapsulate the devnet and testnet Solana environments. Any endpoints or tokens created under a developer Subscription will automatically be usable for both Solana environments, typically at no cost.

## **List subscription types**

<mark style="color:red;">This API is only limited to account management API tokens created with the reseller role.</mark>

<mark style="color:blue;">`GET /api/v1/subscription_types`</mark>

Returns all subscription types that the current authentication token has access to.

**Parameters**

Currently, there are no parameters that would be used.

**Request**

<pre><code><strong>curl 'https://customers.triton.one/api/v1/subscription_types' -H "Authorization: secret-api-token" -H "Content-Type: application/json" -H "Accept:application/json" 
</strong></code></pre>

**Response**

Returns all subscription types that a Subscription can get created with.

```json
{
   "subscription_types": [
      {
         "uuid": "413defdd-b011-4dda-8145-fa782524284c",
         "name": "developer",
         "pool_name": "test pool name",
         "subscriptions_count": 10,
         "endpoint_default_suffixes": [
            "TEST 1",
            "TWST 2"
         ]
      },
      {
         "uuid": "8ddbdbc8-b513-4ba5-a833-2ce8402a9c50",
         "name": "mainnet-shared",
         "pool_name": "pool name",
         "subscriptions_count": 10,
         "endpoint_default_suffixes": [
            "endpoint suffix"
         ]
      },
      {
         "uuid": "b97a3264-2c0a-4af5-85ff-14dce864afc9",
         "name": "mainnet-dedicated",
         "pool_name": "pool name",
         "subscriptions_count": 5,
         "endpoint_default_suffixes": [
            "endpoint suffix"
         ]
      }
   ]
}
```

## **Create subscription type / update subscription type**

<mark style="color:red;">This API is only limited to account management API tokens created with the admin role.</mark>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
