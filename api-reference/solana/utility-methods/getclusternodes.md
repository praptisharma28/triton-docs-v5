---
description: >-
  Returns network endpoints, shred version, feature set, and software version
  for known cluster nodes.
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

# getClusterNodes

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getClusterNodes"
}'
```
{% endtab %}

{% tab title="Kit" %}
```typescript
import { createSolanaRpc } from "@solana/kit";

const rpc_url = "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>";
const rpc = createSolanaRpc(rpc_url);

let nodes = await rpc.getClusterNodes().send();

console.log(nodes);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import { Connection } from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

let nodes = await connection.getClusterNodes();

console.log(nodes);
```
{% endtab %}

{% tab title="Rust" %}
```rust
use anyhow::Result;
use solana_client::nonblocking::rpc_client::RpcClient;
use solana_commitment_config::CommitmentConfig;

#[tokio::main]
async fn main() -> Result<()> {
    let client = RpcClient::new_with_commitment(
        String::from("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>"),
        CommitmentConfig::confirmed(),
    );

    let nodes = client.get_cluster_nodes().await?;

    println!("{:#?}", nodes);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "featureSet": 3073396398,
      "gossip": "10.239.6.48:8001",
      "pubkey": "9QzsJf7LPLj8GkXbYT3LFDKqsj2hHG7TA3xinJHu8epQ",
      "pubsub": "10.239.6.48:8900",
      "rpc": "10.239.6.48:8899",
      "serveRepair": "10.239.6.48:8002",
      "shredVersion": 2405,
      "tpu": "10.239.6.48:8856",
      "tpuForwards": "10.239.6.48:8857",
      "tpuForwardsQuic": "10.239.6.48:8863",
      "tpuQuic": "10.239.6.48:8862",
      "tpuVote": "10.239.6.48:8858",
      "tvu": "10.239.6.48:8000",
      "version": "1.0.0 c375ce1f"
    }
  ],
  "id": 1
}
```

## Result

The result field will be an array of JSON objects, each with the following sub fields:

**`featureSet`** · u32 | null

First 4 bytes of the node's FeatureSet identifier

**`gossip`** · string | null

Gossip network address for the node

**`pubkey`** · string

Node public key, as base-58 encoded string

**`pubsub`** · string | null

WebSocket PubSub network address for the node, or `null` if the PubSub service is not enabled

**`rpc`** · string | null

JSON RPC network address for the node, or `null` if the JSON RPC service is not enabled

**`serveRepair`** · string | null

Serve repair UDP network address for the node

**`shredVersion`** · u16 | null

The shred version the node has been configured to use

**`tpu`** · string | null

TPU UDP network address for the node

**`tpuForwards`** · string | null

TPU forwards UDP network address for the node

**`tpuForwardsQuic`** · string | null

TPU forwards QUIC network address for the node

**`tpuQuic`** · string | null

TPU QUIC network address for the node

**`tpuVote`** · string | null

TPU vote UDP network address for the node

**`tvu`** · string | null

TVU UDP network address for the node

**`version`** · string | null

The software version of the node, or `null` if the version information is not available

***

_Adapted from the_ [_Solana `getClusterNodes` reference_](https://solana.com/docs/rpc/http/getclusternodes)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
