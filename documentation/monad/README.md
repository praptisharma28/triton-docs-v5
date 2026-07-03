---
description: >-
  Monad on Triton, with JSON-RPC and WebSocket APIs, block explorers, and
  examples.
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

# Overview

Monad is a high-performance, EVM-compatible Layer 1. Triton runs Monad RPC endpoints for early builders, with the standard Ethereum JSON-RPC and WebSocket APIs, so your existing EVM tooling works unchanged.

## Block explorers

* Mainnet: [https://monadvision.com](https://monadvision.com)
* Testnet: [https://testnet.monadvision.com/](https://testnet.monadvision.com/)

## Developer docs

* [https://docs.monad.xyz](https://docs.monad.xyz/)

## APIs

{% hint style="warning" %}
The differences between Monad and the standard Geth client are described here: [https://docs.monad.xyz/reference/rpc-differences](https://docs.monad.xyz/reference/rpc-differences)
{% endhint %}

## JSON-RPC

Monad supports most of the JSON-RPC methods available in Geth. You can find the official documentation [here](https://docs.monad.xyz/reference/json-rpc/)

The linked documentation also includes cURL examples where you just need to replace the RPC URL, e.g:

```bash
curl --request POST \
     --url <TRITON_URL> \
     --header 'accept: application/json' \
     --header 'content-type: application/json' \
     --data '
{
  "id": 0,
  "jsonrpc": "2.0",
  "method": "eth_blockNumber",
  "params": []
}
'
```

Expected result:

```bash
{
    "jsonrpc": "2.0",
    "result": "0x23feb61",
    "id": 0
}
```

## WebSocket

The WebSocket API is also available in Monad. You can find the official documentation [here](https://docs.monad.xyz/reference/websockets)

See an example below how to test WebSocket using the `wscat` utility:

**Install node-ws in Linux**:

```bash
sudo apt update
sudo apt install node-ws
```

**Subscribe to websockets**:

```bash
wscat -c wss://<TRITON_URL>
Connected (press CTRL+C to quit)
> { "id": 1, "jsonrpc": "2.0", "method": "eth_subscribe", "params": ["newHeads"] }
```

Expected result:

```json
{"jsonrpc": "2.0", "result": "0x42cef98808e973c28e55c2ecd59c85c5", "id": 1}
```

Subsequent messages stream each new block header:

```json
{
  "jsonrpc": "2.0",
  "method": "eth_subscription",
  "params": {
    "subscription": "0x42cef98808e973c28e55c2ecd59c85c5",
    "result": {
      "hash": "0x3b4e76a0bcfcc7b6f6eb0c44ca8d95f59ae3feb9fb378922b5e9697beee6178e",
      "parentHash": "0x68f6ed9d35ad80209efd9c6a8c23557da367a424233ef15b6c9a562544fa059e",
      "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
      "miner": "0x63ce5e9d70b77c60ccc68158eafe6398c9ce61f8",
      "stateRoot": "0x5d232d542a1beee75e0803af58d34f5d6305ea8c6c6da01535d793b54e300590",
      "transactionsRoot": "0x8bcc011db89df7ec8f35b55fdc6f4d0fe3823e6b08df32f088b318348225b71d",
      "receiptsRoot": "0xfcde62dec790860f76d45e98f50df4878b6880860305c8b3990d5432535d6b09",
      "logsBloom": "0x242c804d010488e0...12000080",
      "difficulty": "0x0",
      "number": "0x23fee0f",
      "gasLimit": "0xbebc200",
      "gasUsed": "0x203788b",
      "timestamp": "0x6924aa80",
      "extraData": "0x0000000000000000000000000000000000000000000000000000000000000000",
      "mixHash": "0x753243a7ce528c3925b31dc1b6c33fc784e6a68ebc2dedbbef8b4c53fe25de66",
      "nonce": "0x0000000000000000",
      "baseFeePerGas": "0x174876e800",
      "withdrawalsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
      "blobGasUsed": "0x0",
      "excessBlobGas": "0x0",
      "parentBeaconBlockRoot": "0x0000000000000000000000000000000000000000000000000000000000000000",
      "requestsHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
      "totalDifficulty": "0x0",
      "size": "0x30f"
    }
  }
}
```

_Note: JS examples are also available in the official documentation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
