---
description: Return the list of dynamic field objects owned by an object.
---

# suix\_getDynamicFields

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_getDynamicFields",
  "params": [
    "0x5612581eba57ebe7e594b809ccceec2be4dac6ff6945d49b3ecc043d049611f6",
    "0x671832358f25bfacde706e528df4e15bb8de6dadd21835dfe44f4973139c15f9",
    3
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter          | Type     | Required | Description                                                                                                                                                        |
| ------------------ | -------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `parent_object_id` | ObjectID | Yes      | The ID of the parent object                                                                                                                                        |
| `cursor`           | ObjectID | No       | An optional paging cursor. If provided, the query will start from the next item after the specified cursor. Default to start from the first item if not specified. |
| `limit`            | integer  | No       | Maximum item returned per page, default to \[QUERY\_MAX\_RESULT\_LIMIT] if not specified.                                                                          |

## Response

Returns `DynamicFieldPage`. (`Page_for_DynamicFieldInfo_and_ObjectID`)

```json
{
  "data": [
    {
      "name": {
        "type": "0x0000000000000000000000000000000000000000000000000000000000000009::test::TestField",
        "value": "some_value"
      },
      "bcsEncoding": "base64",
      "bcsName": "FDB4OTo6dGVzdDo6VGVzdEZpZWxk",
      "type": "DynamicField",
      "objectType": "test",
      "objectId": "0xcfd10bca4d517e9452ad5486d69ee482b758c2399039dbbedd5db24385e934d6",
      "version": 1,
      "digest": "9oCJR2QHVThbwWtSYwmWv6oSFw26PuxXkLyFrUbNqpU2"
    },
    {
      "name": {
        "type": "0x0000000000000000000000000000000000000000000000000000000000000009::test::TestField",
        "value": "some_value"
      },
      "bcsEncoding": "base64",
      "bcsName": "FDB4OTo6dGVzdDo6VGVzdEZpZWxk",
      "type": "DynamicField",
      "objectType": "test",
      "objectId": "0x05a4a796534a1833ca2c4df8fda7d073bbbf2715d2cd82ed40dc051dd5e05f7f",
      "version": 1,
      "digest": "3F8njMJQe6DNxeuvUnHPVjuR9Lt3RNwfsBoxDcB9SXAa"
    },
    {
      "name": {
        "type": "0x0000000000000000000000000000000000000000000000000000000000000009::test::TestField",
        "value": "some_value"
      },
      "bcsEncoding": "base64",
      "bcsName": "FDB4OTo6dGVzdDo6VGVzdEZpZWxk",
      "type": "DynamicField",
      "objectType": "test",
      "objectId": "0x6d95af2033dd243fe6bdc6886d51b7d1cb695b9491893f88a5ae1b9d4f235b3c",
      "version": 1,
      "digest": "9Ury7TXnLtHDrxreKjv5eMJpDAU4wZRuev4JJ1UnJBMp"
    }
  ],
  "nextCursor": "0xfd0b2c4326c56b1fec231d73038dba0f0885b97982f5fcac3ec6f5c8cae16743",
  "hasNextPage": true
}
```

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
