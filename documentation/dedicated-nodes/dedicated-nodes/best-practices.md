# Best practices

Get the most out of a dedicated streaming node.

* **Put all your streaming traffic on the dedicated endpoint.** Leaving streams on a shared endpoint means metered charges and defeats the point of a fixed-cost node.
* **Run your backend close to the node.** gRPC streams are backend-to-backend, so geographic distance is the main latency factor. Place your service in the same region as your node rather than relying on geo-distribution.
* **Use the regional endpoint** where a product has one. Fumarole, for example, runs independent regional clusters, so connect to the regional hostname rather than the shared geo-routed one.
* **Handle dropped streams.** Networks drop. Reconnect and resume from your last-seen slot with Dragon's Mouth replay (`from_slot`), or use [Fumarole](../../real-time-streaming/fumarole-persistent-streams.md), whose server-side cursor picks up where you left off. After a brief disconnect you can also backfill missed updates with a `getBlock` call.
* **Filter to cut volume.** Server-side filters (skip votes, restrict to the accounts and programs you care about) reduce both bandwidth and cost. See [streaming best practices](../../real-time-streaming/streaming-data/best-practices.md).
* **Failover is automatic.** Nodes fail over into a backup pool, so a single node restarting or briefly falling behind the chain tip does not interrupt your stream.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
