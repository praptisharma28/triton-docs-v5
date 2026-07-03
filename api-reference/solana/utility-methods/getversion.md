---
description: >-
  Returns the node's software version string and optional feature set
  identifier.
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

# getVersion

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getVersion"
}'
```
{% endtab %}

{% tab title="Kit" %}
```typescript
import { createSolanaRpc } from "@solana/kit";

const rpc_url = "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>";
const rpc = createSolanaRpc(rpc_url);

let version = await rpc.getVersion().send();

console.log(version);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import { Connection } from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

let version = await connection.getVersion();

console.log(version);
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

    let version = client.get_version().await?;

    println!("{}", version);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response

```json
{
  "jsonrpc": "2.0",
  "result": {
    "solana-core": "3.1.8",
    "feature-set": 2891131721
  },
  "id": 1
}
```

## Result

Version response object containing:

**`solana-core`** · string

Node software version string.

**`feature-set`** · u32 | null

First four bytes of the current software's FeatureSet identifier. This may be `null` if the node does not report a feature set.

***

_Adapted from the_ [_Solana `getVersion` reference_](https://solana.com/docs/rpc/http/getversion)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
