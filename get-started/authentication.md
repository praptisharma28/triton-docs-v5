---
description: Authenticate securely, protecting your endpoint and credentials.
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

# Auth and security

Triton endpoints support two authentication methods, each for a different context. Using the wrong one is the most common security mistake we see.

## Secret tokens (backend)

{% hint style="danger" %}
Never put it in frontend code. If it is embedded in a browser-facing app, anyone who opens DevTools can read it, and any requests they send land on your bill.
{% endhint %}

A token authenticates requests from your backend: servers, scripts, trading bots, gRPC clients. It identifies your account and unlocks higher rate limits. You attach it in one of 2 ways:

### Token in the URL path

JSON-RPC and WebSocket accept the token in the URL path.

```
https://<your-endpoint>.mainnet.rpcpool.com/<your-token>
wss://<your-endpoint>.mainnet.rpcpool.com/<your-token>
```

gRPC rejects a token in the URL path with a `403`. Send it in an `x-token: <your-token>` header instead, keeping the URL clean.

```
https://<your-endpoint>.mainnet.rpcpool.com
```

### x-token header

The `x-token` metadata header can authenticate every request type: JSON-RPC, WebSocket, and gRPC.

```
x-token: <your-token>
https://<your-endpoint>.mainnet.rpcpool.com
```

```
x-token: <your-token>
wss://<your-endpoint>.mainnet.rpcpool.com
```

## Allowed origins (frontend)

Browser apps use the bare endpoint URL with no token, over HTTPS or WebSocket (`wss://`), and you whitelist the domains allowed to call it in the customer dashboard:

```
https://<your-endpoint>.mainnet.rpcpool.com
```

Triton serves requests only from the origins you list, rate-limited by IP and origin domain.

{% hint style="info" %}
**To add allowed origins:**

1. Open the [customer dashboard](https://customers.triton.one/)
2. Click the subscription, then the endpoint
3. Click the `+` next to allowed origins
4. Add the domain and save
{% endhint %}

When you open the allowed origins list, you'll see `__blocked.rpcpool.com`. Don't remove it.

It is a system origin that keeps your endpoint closed to unauthenticated requests. If you remove it and leave the list empty, the endpoint becomes fully open: anyone can query it without a token.

**Always use HTTPS.** Endpoints work over HTTP (port 80), but unencrypted requests expose your token and data to network snooping. Use `https://` everywhere.

## Endpoint hygiene

A short checklist that meaningfully reduces your exposure:

* **Disable endpoints you're not using.** Provisioned one for a one-off backfill that's still active? Clean it up in the dashboard.
* **Set a spend cap with prepaid credit.** A PAYG balance acts as a natural cut-off for abuse. Set low-balance alerts in the Billing tab.
* **Monitor RPC metrics.** Spikes in unfamiliar methods, traffic at unusual hours, or sudden bandwidth jumps are worth investigating.
* **Don't log tokens.** Tokens often end up in application logs and observability pipelines. Treat them like a database password.
* **Separate tokens per environment.** Your Triton account ships with both mainnet and devnet tokens. Use distinct tokens across production, staging, and development to limit the blast radius of a leak.

## If a token leaks

If a token ends up in a public GitHub repo, a frontend bundle, a committed config file, or a shared message, act fast:

1. **Contact support immediately.** Use the chat in your [customer dashboard](https://customers.triton.one/). We'll rotate the token and issue a new one. The old token becomes invalid as soon as the new one is issued.
2. **Check your billing dashboard.** Look at the RPC Metrics tab for unusual traffic spikes or unfamiliar methods since the suspected leak.
3. **Audit your codebase.** Search for the token string across all repos, config files, CI/CD pipelines, and environment files. GitHub secret scanning helps automate this.
4. **Review your allowed origins.** Make sure the list is tight, no wildcards, no overly broad entries.
5. **Rotate related credentials.** If the token was reused across environments or stored alongside other secrets, treat those as compromised too.

## Why we advise against proxying

Putting a third-party proxy (Cloudflare or similar) in front of your Triton endpoint typically undermines performance, breaks security guarantees, and adds operational complexity. We strongly advise against it.

Before implementing a proxy, contact support. We can almost always provide a more direct, performant solution.

### Performance impact

* **Breaks the routing.** Our routing connects users to the closest data centre. A proxy that isn't geographically distributed (or that doesn't pass accurate EDNS info) breaks this, removing the latency benefit.
* **Incompatible with BGP Anycast.** Anycast uses the user's real IP to find the optimal path to our network. A proxy masks the real IP, making Anycast routing impossible.
* **Delays failover.** Proxies often cache DNS records, interfering with our ability to reroute during emergencies. Result: reduced redundancy, longer downtime.

### Security risks

* **Redundant DDoS protection.** Triton already handles DDoS mitigation. A commercial provider on top adds nothing that your endpoint doesn't already have.
* **You become a man-in-the-middle.** A proxy decrypts all traffic between users and our servers. You inherit full responsibility for any resulting incident or data leak.
* **SSL/TLS misconfiguration is common.** "Flexible" or "Full without strict" SSL modes leave parts of the network unencrypted, breaking end-to-end security. Don't disable SSL validation.
* **Abuse prevention shifts to you.** Our systems see all traffic from one proxy IP, so we partially disable our standard rate limits and abuse defences for that endpoint. You take over the responsibility.

### Technical complexity

* **Header forwarding.** Your proxy must correctly forward `Host`, `Origin` (for CORS and routing), and `X-Forwarded-For` / `X-Real-IP` (to pass the original user IP).
* **Specialised token required.** Contact us for a proxy-specific token, and ensure it doesn't leak into proxy error pages or logs.
* **DNS-layer proxies aren't supported.** Simple DNS-layer proxies (e.g. Cloudflare's default proxied DNS records) don't work with shared endpoints. Only full HTTP-layer proxies are workable, and only with the configuration above.

If a misconfigured proxy causes abusive traffic, we'll be forced to severely limit or block your endpoint.

## Contact

* General security or privacy questions: contact support by clicking the chat icon in the bottom right of your [customer dashboard](https://customers.triton.one)
* Urgent security reports: [noc@triton.one](mailto:noc@triton.one)

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-laptop">:laptop:</i> <strong>Platform overview</strong></td><td>Where to get your endpoint and token, monitor usage, and top up your balance.</td><td><a href="https://app.gitbook.com/s/ACym6ZbIwDBDKhyKgDGy/platform-overview">https://app.gitbook.com/s/ACym6ZbIwDBDKhyKgDGy/platform-overview</a></td></tr><tr><td><i class="fa-shield-halved">:shield-halved:</i> <strong>Privacy and data protection</strong></td><td>Triton's defence-in-depth security model and how your data is protected.</td><td><a href="https://app.gitbook.com/s/ACym6ZbIwDBDKhyKgDGy/privacy-and-security">https://app.gitbook.com/s/ACym6ZbIwDBDKhyKgDGy/privacy-and-security</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
