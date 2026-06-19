---
description: >-
  Build unsigned transactions server-side with the unsafe_* methods. Most
  applications should build transactions with the Sui SDK instead.
---

# Transaction builder API

Server-side builders that return an unsigned transaction for common operations: transfers, coin splits and merges, staking, Move calls, and publishing.

These are the `unsafe_*` methods. Most applications should build transactions with the Sui SDK instead, and use these only when building transactions on the server.
