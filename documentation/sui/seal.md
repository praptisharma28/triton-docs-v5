---
description: >-
  Triton-operated Seal key servers on Sui, for decentralised secrets and
  threshold decryption.
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

# Seal

[Seal](https://github.com/MystenLabs/seal) is Mysten Labs' decentralised secrets management for Sui. It provides client-side encryption with secret sharing across multiple key servers, with access enforced by on-chain Move policies. Encrypted data can be stored on Walrus or any other storage, and decryption uses t-out-of-n threshold decryption across independent key servers.

Triton operates Seal key servers on Sui mainnet and testnet as a shared service.

## Key servers

Triton runs two kinds of Seal key server:

* **Open server.** Serves key requests for any package, usable with any Triton Sui subscription.
* **Permissioned server.** Serves only packages that have been registered with it. Contact support to register a package.

| Server       | Network | Endpoint                              |
| ------------ | ------- | ------------------------------------- |
| Open         | Testnet | `seal.testnet.sui.rpcpool.com`        |
| Permissioned | Mainnet | `sealprivate.mainnet.sui.rpcpool.com` |
| Permissioned | Testnet | `sealprivate.testnet.sui.rpcpool.com` |

## Committee key server

Triton is a founding operator in Seal's decentralised key-server committee on Sui mainnet, running one of the threshold key servers that back Seal's multi-party setup. The committee key server is available at:

```
sealkeyservercommittee0.mainnet.sui.rpcpool.com
```

Requests authenticate with an `X-API-KEY` header.

## Access

Seal is available on every Triton Sui subscription. Use the token from your [customer dashboard](https://customers.triton.one). For the protocol, SDKs, and how to encrypt and decrypt with Seal, see Mysten Labs' [Seal documentation](https://github.com/MystenLabs/seal).

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
