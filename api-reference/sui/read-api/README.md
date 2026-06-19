---
description: Read objects, transactions, events, checkpoints, and protocol state.
---

# Read API

Read on-chain state: objects and their contents, transaction blocks, events, checkpoints, and protocol configuration.

Use these methods to fetch a single object or transaction by ID, batch-read many at once with the `multiGet` methods, and look up past versions of an object. `sui_getChainIdentifier` returns the chain identifier, and `sui_verifyZkLoginSignature` verifies a zkLogin signature.
