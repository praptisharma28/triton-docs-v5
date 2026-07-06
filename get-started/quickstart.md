---
description: Create Triton account and send your first request to the network.
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

# Quickstart

{% stepper %}
{% step %}
**Sign up and fund your account**

Create your account at [customers.triton.one](https://customers.triton.one) and activate it with a stablecoin deposit (minimum $125; valid for 12 months). This gives you access to all features across all chains, used up as you go (see Plans and billing for exact rates).
{% endstep %}

{% step %}
**Get your endpoint**

Your endpoints are already provisioned. In the [customer portal](https://customers.triton.one), select the network, then click the endpoint name to copy its URL and secret token (see [Authentication](https://app.gitbook.com/s/ACym6ZbIwDBDKhyKgDGy/authentication) for details).
{% endstep %}

{% step %}
**Send your first request**

The example below calls `getSlot` on Solana mainnet. Paste in your endpoint and secret token, then run it:

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
{% endstep %}
{% endstepper %}

## Start building

Once you have an account, select the chain you're integrating to jump to its docs:

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><i class="fa-bolt">:bolt:</i> <strong>Solana</strong></td><td>RPC, streaming, history, and trading APIs for Solana Mainnet and Devnet</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana">Solana</a></td></tr><tr><td><i class="fa-droplet">:droplet:</i> <strong>Sui</strong></td><td>gRPC, Seal, Walrus, and ledger endpoints for Sui Mainnet and Testnet</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/sui">Sui</a></td></tr><tr><td><i class="fa-cube">:cube:</i> <strong>Monad</strong></td><td>RPC and Websocket endpoints for Monad Mainnet and Testnet</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/monad">Monad</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
