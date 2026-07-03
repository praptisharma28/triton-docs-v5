---
description: Generate a Yellowstone Vixen parser from an IDL using Codama.
---

# Generate parsers with Codama

This guide generates a [Vixen](https://github.com/rpcpool/yellowstone-vixen) parser using [Codama](https://github.com/codama-idl/codama), a tool that renders Rust SDKs and parser implementations from IDLs.

Vixen is a framework for building real-time program data pipelines in Rust. The parser you scaffold here decodes and processes one program's data inside the Vixen runtime.

## Prerequisites

1. An `idl.json` file, either Anchor-generated or custom.
2. [pnpm](https://pnpm.io/) installed (npm or yarn work too).
3. An initialised JavaScript project:

   ```bash
   pnpm init
   ```

## Installation

Install the Codama Vixen parser renderer:

```bash
pnpm install @codama/renderers-vixen-parser
```

Then the dependencies the generation script needs:

```bash
pnpm install \
  @codama/nodes \
  @codama/nodes-from-anchor \
  @codama/renderers-core \
  @codama/visitors-core
```

## Create the generation script

Create a file named `codama.cjs`:

```javascript
const path = require("node:path");
const { rootNode } = require("@codama/nodes");
const { rootNodeFromAnchor } = require("@codama/nodes-from-anchor");
const { readJson } = require("@codama/renderers-core");
const { visit } = require("@codama/visitors-core");
const { renderVisitor } = require("@codama/renderers-vixen-parser");

const projectName = "example-parser";
const idl = readJson(path.join(__dirname, "idl.json"));
const node = rootNodeFromAnchor(idl);

visit(
    node,
    renderVisitor({
        projectFolder: __dirname,
        projectName,
    }),
);
```

## Run the script

```bash
node codama.cjs
```

The generated folder structure:

```
example-parser/
├── proto/
│   └── example_parser.proto
├── src/
│   ├── generated_parser/
│   ├── generated_sdk/
│   └── lib.rs
├── build.rs
├── Cargo.toml
├── codama.cjs
└── idl.json
```

## Build and verify

```bash
cargo build
```

A successful build gives you a working Vixen parser for your program's account data, ready to drop into a Vixen pipeline.

## What's next

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>How to use Vixen + gRPC</strong></td><td>Run your parser against a live Yellowstone gRPC stream.</td><td><a href="https://app.gitbook.com/s/TpqU5Dqc6tdzY8J23dd7/solana/streaming/how-to-use-vixen-grpc">How to use Vixen + gRPC</a></td></tr><tr><td><i class="fa-tower-broadcast">:tower-broadcast:</i> <strong>Dragon's Mouth gRPC</strong></td><td>Sub-slot real-time updates for accounts, transactions, slots, and blocks via gRPC.</td><td><a href="https://app.gitbook.com/s/Xz3Ki4zincxsnRG91NNt/solana/real-time-streaming/dragon-s-mouth-grpc">Dragon's Mouth gRPC</a></td></tr></tbody></table>

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
