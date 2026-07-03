---
description: Returns the current epoch's effective inflation rates.
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

# getInflationRate

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>" -s -X POST \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getInflationRate"
}'
```
{% endtab %}

{% tab title="Kit" %}
```typescript
import { createSolanaRpc } from "@solana/kit";

const rpc_url = "https://<your-endpoint>.mainnet.rpcpool.com/<your-token>";
const rpc = createSolanaRpc(rpc_url);

let inflationRate = await rpc.getInflationRate().send();

console.log(inflationRate);
```
{% endtab %}

{% tab title="web3.js" %}
```typescript
import { Connection } from "@solana/web3.js";

const connection = new Connection("https://<your-endpoint>.mainnet.rpcpool.com/<your-token>", "confirmed");

let inflationRate = await connection.getInflationRate();

console.log(inflationRate);
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

    let inflation_rate = client.get_inflation_rate().await?;

    println!("{:#?}", inflation_rate);

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
    "total": 0.149,
    "validator": 0.148,
    "foundation": 0.001,
    "epoch": 100
  },
  "id": 1
}
```

## Result

The result field will be a JSON object with the following fields:

**`total`** · f64

Total inflation

**`validator`** · f64

Inflation allocated to validators

**`foundation`** · f64

Inflation allocated to the foundation

**`epoch`** · u64

Epoch for which these values are valid

***

_Adapted from the_ [_Solana `getInflationRate` reference_](https://solana.com/docs/rpc/http/getinflationrate)_, maintained by the Solana Foundation._

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
