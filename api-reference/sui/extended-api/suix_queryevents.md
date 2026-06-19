---
description: Return list of events for a specified query criteria.
---

# suix\_queryEvents

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.sui.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "suix_queryEvents",
  "params": [
    {
      "MoveModule": {
        "package": "0xa395759ca37c6e1ffc179184e98a6f9a2da5d78f6e34b0e5044ed52a6bc0a1bc",
        "module": "test"
      }
    },
    {
      "txDigest": "Eg3ynETJfTfPKyvJzq3VLG6MngURYHPMjjUJ3Xt1t7tf",
      "eventSeq": "1"
    },
    100,
    false
  ]
}'
```
{% endtab %}
{% endtabs %}

## Parameters

| Parameter          | Type        | Required | Description                                                                                                                 |
| ------------------ | ----------- | -------- | --------------------------------------------------------------------------------------------------------------------------- |
| `query`            | EventFilter | Yes      | The event query criteria. See [Event filter](https://docs.sui.io/build/event_api#event-filters) documentation for examples. |
| `cursor`           | EventID     | No       | optional paging cursor                                                                                                      |
| `limit`            | integer     | No       | maximum number of items per page, default to \[QUERY\_MAX\_RESULT\_LIMIT] if not specified.                                 |
| `descending_order` | boolean     | No       | query result ordering, default to false (ascending order), oldest record first.                                             |

## Response

Returns `EventPage`. (`Page_for_Event_and_EventID`)

```json
{
  "data": [
    {
      "id": {
        "txDigest": "FFwCMgC7FHBLEwfL9JeSeR2EhMAZMykUPVW1kE3HgTMe",
        "eventSeq": "1"
      },
      "packageId": "0xb2fd632992b01aa25900867288b63d6255ff8223c12b0fd985c49d5777a0d65a",
      "transactionModule": "test",
      "sender": "0xcceee09f44d558691334ec0aff47af033f57162a2f33056e2585e2c46863ac02",
      "type": "0x3::test::Test<0x3::test::Test>",
      "parsedJson": "some_value",
      "bcsEncoding": "base64",
      "bcs": ""
    },
    {
      "id": {
        "txDigest": "FUMhRSj76es8MYeaRYeaBnppk56cuEehKwL2CiU82U7B",
        "eventSeq": "1"
      },
      "packageId": "0xb2fd632992b01aa25900867288b63d6255ff8223c12b0fd985c49d5777a0d65a",
      "transactionModule": "test",
      "sender": "0x84bd999f9ff7a1804872957fafa528628a24386298faa98850887f64da841b87",
      "type": "0x3::test::Test<0x3::test::Test>",
      "parsedJson": "some_value",
      "bcsEncoding": "base64",
      "bcs": ""
    },
    {
      "id": {
        "txDigest": "CkEYWW2zxTCGBLvUcTARhyX92fu2uc7cnCUXfCiqAypp",
        "eventSeq": "1"
      },
      "packageId": "0xb2fd632992b01aa25900867288b63d6255ff8223c12b0fd985c49d5777a0d65a",
      "transactionModule": "test",
      "sender": "0x279efd098d59a66a3d9adc87cce81fe9ec69dc8105b2b60140589ec8be44c29f",
      "type": "0x3::test::Test<0x3::test::Test>",
      "parsedJson": "some_value",
      "bcsEncoding": "base64",
      "bcs": ""
    },
    {
      "id": {
        "txDigest": "Eg3ynETJfTfPKyvJzq3VLG6MngURYHPMjjUJ3Xt1t7tf",
        "eventSeq": "1"
      },
      "packageId": "0xb2fd632992b01aa25900867288b63d6255ff8223c12b0fd985c49d5777a0d65a",
      "transactionModule": "test",
      "sender": "0x289be027d2a94f744b4c59fda7b528f9c59f430eaba84b8bee9b43a30f9cc83f",
      "type": "0x3::test::Test<0x3::test::Test>",
      "parsedJson": "some_value",
      "bcsEncoding": "base64",
      "bcs": ""
    }
  ],
  "nextCursor": {
    "txDigest": "Eg3ynETJfTfPKyvJzq3VLG6MngURYHPMjjUJ3Xt1t7tf",
    "eventSeq": "1"
  },
  "hasNextPage": false
}
```
