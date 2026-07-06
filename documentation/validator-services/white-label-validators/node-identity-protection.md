---
description: How Triton safeguards the node identity key while you keep control of your validator.
---

# Node identity protection

The public-private key pair of the node identity enables the node to vote on behalf of your vote account. We hold this key pair and deploy it to the node.

This key pair's account must have a sufficient balance to pay voting fees. This key also accrues rewards that help to offset vote fees (or entirely cover them, depending on stake). Therefore, the balance of the node identity may need to be maintained and topped up regularly.

The node identity must be kept safe and secure. Anyone possessing the node identity could set up a second validator that creates issues for your primary validator (and, in the future, potentially slashing). Malicious parties could also receive the rewards the key accrues or empty its balance.

To protect the node identity, we adopt a setup that ensures in-memory-only keys. When our validators boot, they use a temporary node identity that cannot vote for your vote account. This means that once the validator has started up and caught up with the network, it will be in voting-disabled mode.

One of our node operators will then authenticate with our secured, encrypted secret storage and enable the validator node to perform a one-time fetch of the real node identity, which is then activated in the Solana validator's memory.

This ensures that the node identity is never stored in an unencrypted form anywhere. It moves only between our encrypted storage and the validator node, over an authenticated, encrypted stream.

Without access to the validator memory space, you would be unable to retrieve the node identity, and there is no durable storage of the identity on the node itself.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
