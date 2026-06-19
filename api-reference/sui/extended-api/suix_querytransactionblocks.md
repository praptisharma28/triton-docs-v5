---
description: Return list of transactions for a specified query criteria.
---

# suix\_queryTransactionBlocks

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_queryTransactionBlocks",
  "params": [
    {
      "filter": {
        "InputObject": "0x93633829fcba6d6e0ccb13d3dbfe7614b81ea76b255e5d435032cd8595f37eb8"
      },
      "options": null
    },
    "HxidAfFfyr4kXSiWeVq1J6Tk526YUVDoSUY5PSnS4tEJ",
    100,
    false
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter          | Type                          | Required | Description                                                                                                                                                        |
| ------------------ | ----------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `query`            | TransactionBlockResponseQuery | Yes      | the transaction query criteria.                                                                                                                                    |
| `cursor`           | TransactionDigest             | No       | An optional paging cursor. If provided, the query will start from the next item after the specified cursor. Default to start from the first item if not specified. |
| `limit`            | integer                       | No       | Maximum item returned per page, default to QUERY\_MAX\_RESULT\_LIMIT if not specified.                                                                             |
| `descending_order` | boolean                       | No       | query result ordering, default to false (ascending order), oldest record first.                                                                                    |

## Response

Returns `TransactionBlocksPage`. (`Page_for_TransactionBlockResponse_and_TransactionDigest`)

```json
{
  "data": [
    {
      "digest": "GUPcK4cmRmgsTFr52ab9f6fnzNVg3Lz6hF2aXFcsRzaD"
    },
    {
      "digest": "B2iV1SVbBjgTKfbJKPQrvTT6F3kNdekFuBwY9tQcAxV2"
    },
    {
      "digest": "8QrPa4x9iNG5r2zQfmeH8pJoVjjtq9AGzp8rp2fxi8Sk"
    },
    {
      "digest": "3nek86HEjXZ7K3EtrAcBG4wMrCS21gqr8BqwwC6M6P7F"
    }
  ],
  "nextCursor": "3nek86HEjXZ7K3EtrAcBG4wMrCS21gqr8BqwwC6M6P7F",
  "hasNextPage": false
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
