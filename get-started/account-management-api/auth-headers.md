---
description: >-
  Authenticate account management API requests by passing your token in the
  Authorization header.
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

# Auth & headers

## **Auth**

Pass your token in through the Authorization header to make scoped, authenticated requests against the account management API. You can use the `GET /api/v1/accounts` endpoint to test your token. If you get a 200 status code response, your token is being passed correctly. This token is only used for the account management API (`customers.rpcpool.com/api`). You'll create separate tokens for consuming the RPC API.

Examples in this account management API guide are demonstrated using curl, but you should use an HTTP library of your choice.

```
$ curl -H "Authorization: secret" -H "Content-Type: application/json" -H "Accept:application/json" https://customers.triton.one/api/v1/accounts         
```

## **Headers**

All endpoints accept and return JSON, and you should pass both an Accept and Content-Type header of `application/json`

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
