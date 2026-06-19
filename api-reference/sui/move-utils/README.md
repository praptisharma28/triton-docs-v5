---
description: Inspect the structure of on-chain Move packages, modules, functions, and structs.

---

# Move utils

These return a structured representation of a deployed module or its whole package:

| Method | What it returns |
| ------ | --------------- |
| `sui_getNormalizedMoveModule` | A structured representation of a Move module. |
| `sui_getNormalizedMoveModulesByPackage` | Structured representations of every module in a package. |

These describe the functions and structs inside a module:

| Method | What it returns |
| ------ | --------------- |
| `sui_getNormalizedMoveFunction` | A structured representation of a Move function. |
| `sui_getNormalizedMoveStruct` | A structured representation of a Move struct. |
| `sui_getMoveFunctionArgTypes` | The argument types of a Move function. |

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
