---
description: Direct HTTP endpoint for submitting Solana transactions through Cascade without JSON-RPC overhead.
---

# sendTx

`POST /sendtx` is a direct HTTP transaction submission path on Cascade-enabled Solana endpoints. It takes a serialized transaction over plain HTTP and skips the JSON-RPC layer, removing several sources of latency that a standard [sendTransaction](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/sending-transactions/sendtransaction) call carries. Reach for it on latency-sensitive workloads where every millisecond of submission overhead matters.

## Why use it over sendTransaction

A `sendTransaction` call wraps your transaction in a JSON-RPC envelope, which adds overhead at every stage. `/sendtx` strips that away:

* **No JSON parsing.** The server reads your transaction bytes directly, with no JSON deserialisation.
* **No CORS preflight.** With `Content-Type: application/octet-stream` or `text/plain`, browsers skip the preflight `OPTIONS` request, saving a full round-trip.
* **Smaller payloads.** Without the `jsonrpc`, `id`, `method`, and `params` wrapper, the request body is smaller on the wire.
* **Simpler clients.** A single HTTP `POST`, with no Solana JSON-RPC client library.

This suits browser apps that are sensitive to preflight latency and high-frequency backends sending large volumes. If you need full `sendTransaction` options such as `skipPreflight`, keep using [sendTransaction](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/sending-transactions/sendtransaction).

## Request

| Field  | Value      |
| ------ | ---------- |
| Method | `POST`     |
| Path   | `/sendtx`  |

Send the serialized transaction as the request body, one of two ways:

* **Raw bytes.** Set `Content-Type: application/octet-stream` and send the transaction as a binary payload.
* **Encoded string.** Set `Content-Type: text/plain` and send the transaction as base58 or base64 text, with the `encoding` parameter set to match.

### Query parameters

| Parameter     | Values             | Required    | Description                                                            |
| ------------- | ------------------ | ----------- | --------------------------------------------------------------------- |
| `encoding`    | `base58`, `base64` | No          | Encoding of a text-body transaction. Defaults to `base58`.            |
| `response`    | `signature`        | Recommended | When set, the response body returns the transaction signature on success. |
| `max_retries` | integer            | No          | Override the default retry count for this transaction.                 |

### Optional headers

| Header                     | Description                                                                          |
| -------------------------- | ----------------------------------------------------------------------------------- |
| `Solana-ForwardingPolicies` | Comma-separated [Yellowstone Shield](https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/shield-mev-protection) policy addresses to apply when forwarding. |

## Examples

{% tabs %}
{% tab title="Raw bytes" %}
```bash
curl -X POST 'https://<your-endpoint>/sendtx?response=signature&max_retries=3' \
  -H 'Content-Type: application/octet-stream' \
  --data-binary @transaction.bin
```
{% endtab %}

{% tab title="Base64" %}
```bash
curl -X POST 'https://<your-endpoint>/sendtx?encoding=base64&response=signature' \
  -H 'Content-Type: text/plain' \
  -d '<base64-encoded-transaction>'
```
{% endtab %}

{% tab title="Base64 + Shield policy" %}
```bash
curl -X POST 'https://<your-endpoint>/sendtx?encoding=base64&response=signature' \
  -H 'Solana-ForwardingPolicies: <policy-address>' \
  -d '<base64-encoded-transaction>'
```
{% endtab %}
{% endtabs %}

## Response

On success, the endpoint returns **HTTP 200**. If `response=signature` was set, the body is the transaction signature as plain text. Otherwise the body is empty, and you derive the signature client-side from the signed transaction before submitting, since it is deterministic.

On failure, it returns **HTTP 4xx or 5xx** with error details in the body.

`/sendtx` is submission only: it does not simulate or support other RPC methods. For client-side retries, compute budgets, and competitive [priority fees](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/sending-transactions/getrecentprioritizationfees), apply the same delivery tactics as [sendTransaction](https://app.gitbook.com/s/wAm6H3EekvI7YDDlKRdD/solana/sending-transactions/sendtransaction).

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
