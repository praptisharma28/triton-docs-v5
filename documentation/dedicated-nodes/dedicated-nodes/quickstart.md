---
description: How to scope, provision, and connect a dedicated Triton node.
---

# Quickstart

Dedicated nodes are set up with our team, not self-serve. Here is the path from first contact to a live stream.

## Step 1. Talk to us

Reach out through [contact sales](https://triton.one/contact). There is a short know-your-customer step where we collect your contact and location details.

## Step 2. Scope your node

A discovery call to understand your streaming volume, your regions, and the job the cluster will run (gRPC streaming, DAS, Photon, or indexing). We agree the specs, region, and number of nodes before anything is deployed.

## Step 3. Provisioning

Hardware is ordered to spec and set up as a dedicated subscription in your [customer dashboard](https://customers.triton.one). Because nodes are built to order rather than kept in stock, expect a short lead time. There are no same-day trials.

## Step 4. Connect

Your dedicated node has its own endpoint, `https://<your-endpoint>.mainnet.rpcpool.com`. Point your gRPC client at it exactly as you would any Triton endpoint, using your endpoint URL and token. For client code in TypeScript, Rust, and Go, see [Dragon's Mouth gRPC](../../real-time-streaming/dragon-s-mouth-grpc.md).

Then move all your streaming traffic to the dedicated endpoint, see [best practices](best-practices.md).

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
