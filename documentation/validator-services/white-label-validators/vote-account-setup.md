---
description: How a Solana vote account is created and configured for your validator.
---

# Vote account setup

{% hint style="info" %}
Should we host your validator, we will set up the vote account for you. The information below is for transparency and educational purposes.
{% endhint %}

To set up a validator, three kinds of keys need to be present:

* Node identity key pair.
* Vote key/account (public key only).
* Withdrawal key pair.

## Node identity key pair

The node identity key pair is owned and held by Triton. Clients guarantee a minimum balance of 10 SOL to keep the validator running. If the balance drops below 10 SOL, the client must send SOL directly to the validator. Contact our support to receive the validator public key if you are creating your vote account.

## Withdrawal key pair

The withdrawal key pair is the one that can be used to modify the vote account; it must be kept safe. You use this key pair to withdraw rewards from the vote account. We recommend you keep this in a hardware wallet or cold storage. This should never be a hotkey or used with online/web wallets. Losing the keys to this account may result in significant losses.

To create a withdrawal key pair, you can run the following:

```bash
solana-keygen new -o ~/authorized-withdrawer-keypair.json
```

## Vote account

The final step involves setting up the vote account. You will generate a keypair for the vote account, but once it is configured, only the public key of the vote account matters. The withdrawal keypair is the one that will be used to control the vote account:

```bash
solana-keygen new -o ~/vote-account-keypair.json
solana create-vote-account ~/vote-account-keypair.json <pubkey for node identity> ~/authorized-withdrawer-keypair.json
```

## Further reading

For more documentation about vote account management, we strongly encourage you to read through the documents on Solana's website here:

{% embed url="https://docs.anza.xyz/operations/guides/vote-accounts" %}

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Node identity protection</strong></td><td>How Triton safeguards the node identity key while you keep control of your validator.</td><td><a href="https://kate-6.gitbook.io/triton-one-docs-v5/documentation/solana/validator-services/sol-staking-usdc-pay-outs/node-identity-protection">Node identity protection</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
