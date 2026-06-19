---
description: Execute, dry-run, and developer-inspect transaction blocks.
---

# Write API

Execute and test transaction blocks.

Use `sui_executeTransactionBlock` to submit a signed transaction to the network. Use `sui_dryRunTransactionBlock` or `sui_devInspectTransactionBlock` to preview a transaction's effects, events, and return values without committing it, for example to estimate gas or simulate a call.
