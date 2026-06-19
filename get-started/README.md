---
description: >-
  Premium bare-metal infrastructure for production workloads on Solana, Sui, and
  Monad.
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
    visible: false
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# Welcome to Triton One

This is the official documentation for Triton One's blockchain RPC services. Whether you're building your first dApp or managing a high-frequency trading operation, our goal is to provide you with performant and feature-rich infrastructure.

## Why teams choose us

Your app is only as reliable, fast, and accurate as the provider behind every call it makes to the network. That's why ecosystem's biggest protocols have built on Triton since 2021:

* **Hardware tuned to the job:** read clusters, streaming nodes, historical indexers, and transaction-routing nodes each run on machines configured for their specific role.
* **Global distribution across 10 cities on 3 continents:** GeoDNS-routing with continuous health checks, load balancing, and automatic failover for minimal latency.
* **Engineering support:** every subscription includes a direct support channel from your dashboard, with on-call engineers ready to help you with any issue.
* **Open markets:** Triton does not log, frontrun, backrun, sell or extract value from your order flow.
* **Consistent high capacity.** We run large, overprovisioned clusters per network, sized to absorb network-wide spikes without affecting customers' quality of service.

## Networks we support

We run premium infrastructure clusters for Solana, Sui, and Monad.

{% tabs %}
{% tab title="Solana" %}
<table><thead><tr><th>Network</th><th width="119.1953125" align="center">JSON-RPC</th><th width="123.5" align="center">WebSocket</th><th width="128.99609375" align="center">Yellowstone gRPC</th><th width="109.296875" align="center">Fumarole gRPC</th><th width="126.1484375" align="center">Cloudbreak accounts</th><th width="120.12890625" align="center">Superbank archive</th></tr></thead><tbody><tr><td>Mainnet</td><td align="center">✓</td><td align="center">✓</td><td align="center">✓</td><td align="center">✓</td><td align="center">✓</td><td align="center">✓</td></tr><tr><td>Devnet</td><td align="center">✓</td><td align="center">✓</td><td align="center">✓</td><td align="center">✗</td><td align="center">v1 only</td><td align="center">✓</td></tr></tbody></table>
{% endtab %}

{% tab title="Sui" %}
<table><thead><tr><th>Network</th><th width="128" align="center">JSON-RPC</th><th align="center">gRPC</th><th align="center">Archival gRPC</th><th align="center">Walrus</th><th align="center">Seal</th></tr></thead><tbody><tr><td>Mainnet</td><td align="center">✓*</td><td align="center">✓</td><td align="center">✓</td><td align="center">✓</td><td align="center">✓</td></tr><tr><td>Testnet</td><td align="center">✓*</td><td align="center">✓</td><td align="center">x</td><td align="center">✓</td><td align="center">✓</td></tr></tbody></table>

\*JSON-RPC is being deprecated by Mysten Labs and deactivates 31 July 2026; migrate to gRPC.
{% endtab %}

{% tab title="Monad" %}
| Network | JSON-RPC | WebSockets |
| ------- | :------: | :--------: |
| Mainnet |     ✓    |      ✓     |
| Testnet |     ✓    |      ✓     |
{% endtab %}
{% endtabs %}

## How to get started

These docs are split into five tabs, switchable from the top of any page. When you're ready to integrate, the [Documentation tab](https://app.gitbook.com/o/z7eEj3vw2lWeBtVPERxu/s/Xz3Ki4zincxsnRG91NNt/) walks you through every product across supported chains.

You can start here:

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-credit-card">:credit-card:</i> <strong>Plans and billing</strong></td><td>Understand how pay-as-you-go billing works and estimate your spend</td><td><a href="plans-and-billing.md">plans-and-billing.md</a></td></tr><tr><td><i class="fa-rocket">:rocket:</i> <strong>Solana overview</strong></td><td>Explore Solana RPC, streaming, trading, and historical tooling</td><td><a href="https://app.gitbook.com/o/z7eEj3vw2lWeBtVPERxu/s/Jhkd9LWLrP5SjK8WtYBC/">Solana docs</a></td></tr><tr><td><i class="fa-droplet">:droplet:</i> <strong>Sui overview</strong></td><td>Connect to Sui real-time and archival services with your endpoint</td><td><a href="https://app.gitbook.com/o/z7eEj3vw2lWeBtVPERxu/s/7XTSDkV8e0YYDQch7Krp/">SUI</a></td></tr><tr><td><i class="fa-cube">:cube:</i> <strong>Monad overview</strong></td><td>See what Triton runs on Monad, and how to connect</td><td><a href="https://app.gitbook.com/o/z7eEj3vw2lWeBtVPERxu/s/Cz2cI3eYbcnJyUbl7e85/">Monad</a></td></tr></tbody></table>

## For AI agents

* **Single-file index:** `https://docs.triton.one/llms.txt`. Drop it into Claude Code, Cursor, or Codex for full coverage in one fetch.
* **Any page as Markdown:** append `.md` to the page URL, e.g. `https://docs.triton.one/solana/streaming/dragons-mouth.md`.
* **MCP server:** point your AI agent at `https://docs.triton.one/~gitbook/mcp` to look things up mid-task and pull only the pages it needs.

A native Triton MCP server with advanced functionality is coming soon.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
