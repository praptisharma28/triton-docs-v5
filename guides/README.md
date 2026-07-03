---
description: >-
  Get your endpoint sending requests, then pick your next step by product or use
  case.
---

# Quickstart on Triton

Just got your endpoint? This is a good place to start. You'll send your first RPC request, then pick where to go next based on what you're building.

{% stepper %}
{% step %}
### Sign up and deposit $125

Sign up at [customers.triton.one](https://customers.triton.one/users/sign-up), verify your email, and top up the $125 minimum (stablecoins only).
{% endstep %}

{% step %}
### Set up an endpoint

Your endpoints are already provisioned. In the dashboard, select **Solana mainnet** (or devnet for testing) and click the endpoint name to copy the two things you'll use everywhere:

* **Endpoint URL**: `<your-endpoint>.mainnet.rpcpool.com`
* **Secret token**: a long random string

Keep the token server-side only. Frontend code uses an origin allowlist instead. See [Auth and security](https://app.gitbook.com/s/ACym6ZbIwDBDKhyKgDGy/authentication).
{% endstep %}

{% step %}
### Send your first request

Call `getSlot` to confirm the endpoint is live. Pick your stack:

{% tabs %}
{% tab title="curl" %}
```bash
curl https://<your-endpoint>.mainnet.rpcpool.com/<your-token> \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"getSlot"}'
```
{% endtab %}

{% tab title="TypeScript" %}
```javascript
import { createSolanaRpc } from "@solana/kit";

const rpc = createSolanaRpc("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>");
const slot = await rpc.getSlot().send();
console.log(slot);
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

r = requests.post(
    'https://<your-endpoint>.mainnet.rpcpool.com/<your-token>',
    json={'jsonrpc': '2.0', 'id': 1, 'method': 'getSlot'},
)
print(r.json()['result'])
```
{% endtab %}

{% tab title="Rust" %}
```rust
use solana_client::rpc_client::RpcClient;

let client = RpcClient::new(
    "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>".to_string(),
);
println!("{}", client.get_slot()?);
```
{% endtab %}
{% endtabs %}

### Expected response

A current slot number means you're connected:

```json
{ "jsonrpc": "2.0", "result": 311340987, "id": 1 }
```

Hitting a 401, 429, timeout, or gRPC 403? See [Error handling](https://app.gitbook.com/s/VeEf321LwceAVlSk9USV/error-handling) for the full debug flow.
{% endstep %}
{% endstepper %}

## What's next

Two ways in. Pick the tab that fits.

{% tabs %}
{% tab title="By product (I know what I want)" %}
Each product is purpose-built for one job. Pick the category, then the product.

{% tabs %}
{% tab title="Reading state" %}
<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-bolt">:bolt:</i> <strong>Standard RPC</strong></td><td>Solana JSON-RPC over HTTPS. Every standard method, served from the regional fleet.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state">Standard RPC</a></td></tr><tr><td><i class="fa-database">:database:</i> <strong>Cloudbreak Accounts</strong></td><td>Fast account and token queries from tailored dynamic indexes and hot paths.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/cloudbreak-indexed-accounts">Cloudbreak Accounts</a></td></tr><tr><td><i class="fa-image">:image:</i> <strong>Metaplex DAS API</strong></td><td>Query NFTs, tokens, and compressed assets via the Digital Asset Standard interface.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/metaplex-das-api">Metaplex DAS API</a></td></tr><tr><td><i class="fa-arrows-rotate">:arrows-rotate:</i> <strong>Account Sync</strong></td><td>Streaming-backed local cache for faster and cheaper account reads, via the web3.js-compatible SDK.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/reading-account-state/account-sync">Account Sync</a></td></tr></tbody></table>
{% endtab %}

{% tab title="Streaming" %}
<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-tower-broadcast">:tower-broadcast:</i> <strong>Dragon's Mouth gRPC</strong></td><td>Sub-slot real-time updates for accounts, transactions, slots, and blocks via gRPC.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/dragon-s-mouth-grpc">Dragon's Mouth gRPC</a></td></tr><tr><td><i class="fa-rotate-right">:rotate-right:</i> <strong>Whirligig WebSocket</strong></td><td>Drop-in for native Solana WebSockets, backed by gRPC. The fastest real-time data for frontends.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/whirligig-websockets">Whirligig WebSocket</a></td></tr><tr><td><i class="fa-layer-group">:layer-group:</i> <strong>Fumarole Persistent gRPC</strong></td><td>Redundant streaming layer with 4 days of stored data and built-in cursor resume.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/fumarole-persistent-streams">Fumarole Persistent gRPC</a></td></tr></tbody></table>
{% endtab %}

{% tab title="History" %}
<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-clock-rotate-left">:clock-rotate-left:</i> <strong>Historical data</strong></td><td>Purpose-built historical backend: millisecond reads from genesis across the full ledger, plus gRPC replay of every block.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/historical-data">Historical data</a></td></tr><tr><td><i class="fa-box-archive">:box-archive:</i> <strong>Superbank streams</strong></td><td>Replay bounded slot ranges of history over gRPC with StreamBlocks and StreamTransactions.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/historical-data#how-it-works">Superbank streams</a></td></tr></tbody></table>
{% endtab %}

{% tab title="Sending transactions" %}
<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-paper-plane">:paper-plane:</i> <strong>Jet TPU client</strong></td><td>End-to-end integration guide: direct-to-leader forwarding over QUIC with leader scheduling, connection pooling, and retries built in.</td><td><a href="https://app.gitbook.com/s/TpqU5Dqc6tdzY8J23dd7/solana/sending-transactions/yellowstone-jet-tpu-client">Jet TPU client</a></td></tr><tr><td><i class="fa-arrow-trend-up">:arrow-trend-up:</i> <strong>Priority Fees API</strong></td><td>Smart fee estimation with tail-aware percentiles. Reliable landing without overpaying.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/priority-fees-api">Priority Fees API</a></td></tr><tr><td><i class="fa-code-branch">:code-branch:</i> <strong>Metis Swap API</strong></td><td>Swap routing across 20+ DEXes with exact-out and platform-fee support built in.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/3rd-party-apis/metis-swap-api">Metis Swap API</a></td></tr><tr><td><i class="fa-route">:route:</i> <strong>Titan Swap API</strong></td><td>Streaming quotes and routes via DART live re-optimisation or the Prime API for high-volume desks.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/3rd-party-apis/titan-swap-api">Titan Swap API</a></td></tr><tr><td><i class="fa-box">:box:</i> <strong>Jito Bundle Simulation</strong></td><td>Jito Bundle Simulation through Triton endpoints. Test bundle ordering before submitting.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/sending-transactions/3rd-party-apis/jito-bundles">Jito Bundle Simulation</a></td></tr></tbody></table>
{% endtab %}

{% tab title="Dedicated and validator services" %}
<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-server">:server:</i> <strong>Dedicated gRPC node</strong></td><td>Private node with isolated CPU and unlimited concurrent gRPC connections. For latency-sensitive or heavy streaming workloads.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/dedicated-nodes">Dedicated gRPC node</a></td></tr><tr><td><i class="fa-landmark">:landmark:</i> <strong>White-label validator</strong></td><td>Branded validator with full key separation, zero ops overhead, and high availability.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/validator-services/white-label-validators">White-label validator</a></td></tr></tbody></table>
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="By use case (I know what I'm building)" %}
Pick the kind of app you're building. Each card jumps to the matching setup guide.

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-coins">:coins:</i> <strong>Trading or market making</strong></td><td>Live prices, sub-slot tx landing, anti-MEV. Stack: Dragon's Mouth, Jet, Priority Fees, Shield.</td><td><a href="https://app.gitbook.com/s/TpqU5Dqc6tdzY8J23dd7/solana/use-cases/trading-and-market-making">Trading or market making</a></td></tr><tr><td><i class="fa-mobile-screen">:mobile-screen:</i> <strong>Wallet or consumer app</strong></td><td>Balances, history, NFT portfolio, live updates. Stack: Standard RPC, DAS API, Whirligig.</td><td><a href="https://app.gitbook.com/s/TpqU5Dqc6tdzY8J23dd7/solana/use-cases/building-a-wallet">Wallet or consumer app</a></td></tr><tr><td><i class="fa-ellipsis">:ellipsis:</i> <strong>More coming soon</strong></td><td>More use-case guides are on the way: DeFi, NFT marketplaces, indexers, gaming, and AI agents.</td><td></td></tr></tbody></table>
{% endtab %}
{% endtabs %}

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
