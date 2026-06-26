---
description: A web3.js drop-in that serves Solana account reads from a live stream instead of polling.
---

# Account Sync

Poll Solana accounts faster and cheaper. The Triton SDK serves your reads from a local, stream-fed cache, with a one-line drop-in for `@solana/web3.js`.

## Install

```bash
npm install @triton-one/triton-sdk
```

The SDK exports web3.js types and helpers too, so most apps can import from one package:

```ts
import { Connection, PublicKey } from "@triton-one/triton-sdk";
```

If your app already uses web3.js, the first step is usually this:

```diff
- import { Connection, PublicKey } from "@solana/web3.js";
+ import { Connection, PublicKey } from "@triton-one/triton-sdk";
```

## Connection options

The SDK keeps the normal web3.js `Connection` shape and adds one extra config key: `accountSync`.

Use `accountSync` to choose the streaming transport, stream endpoint, account list, commitment, and cache-miss behaviour.

For the short version:

* Use `transport: "grpc"` in Node apps.
* Use `transport: "ws"` in browser apps.
* See [Pick a transport](account-sync.md#pick-a-transport) for which transport to choose.
* See this section for every option you can pass.

```ts
const connection = new Connection(endpoint, {
  commitment: "confirmed",
  accountSync: {
    transport: "grpc",
    subscriptionEndpoint: endpoint,
    commitment: "confirmed",
    initialAccounts: [],
    autoSubscribeOnMiss: true,
    missTimeoutMs: 5_000,
  },
});
```

| Option                 | Default                           | Meaning                                                                                                                                       |
| ---------------------- | --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| `transport`            | `"ws"` in Node, `"ws"` in browser | Stream transport. Node can use `"grpc"` or `"ws"`. Browser can only use `"ws"`.                                                               |
| `subscriptionEndpoint` | Derived from `endpoint`           | Stream endpoint. Set it when your stream endpoint is different from your RPC endpoint. In most cases, it is the same as your Triton endpoint. |
| `commitment`           | `"confirmed"`                     | Stream commitment: `"processed"`, `"confirmed"`, or `"finalized"`.                                                                            |
| `initialAccounts`      | `[]`                              | Accounts to subscribe to as soon as the connection starts.                                                                                    |
| `autoSubscribeOnMiss`  | `true`                            | If you read an account that is not tracked yet, subscribe to it and wait.                                                                     |
| `missTimeoutMs`        | `5000`                            | How long to wait for account data before returning `null`.                                                                                    |

Choose `missTimeoutMs` carefully. The timeout exists because an account might not exist yet. The SDK does not always return RPC-style `null` immediately, because many apps deterministically derive an account address and want the data as soon as that account is created in realtime.

## Migrate from web3.js

For many apps, migration is small:

1. Install `@triton-one/triton-sdk`.
2. Change imports from `@solana/web3.js` to `@triton-one/triton-sdk`.
3. Add `accountSync` options to your `Connection`.
4. Keep calling `getAccountInfo` and `getMultipleAccountsInfo` like before.
5. Call `close()` when the connection is no longer needed.

Before:

```ts
import { Connection, PublicKey } from "@solana/web3.js";

const accountId = "ping6gwBZx1ccMMFyLgkVSupUmujYrFidEXuNRPq989";
const connection = new Connection("https://api.mainnet-beta.solana.com");
const account = await connection.getAccountInfo(new PublicKey(accountId));
```

After:

```ts
import { Connection, PublicKey } from "@triton-one/triton-sdk";

const accountId = "ping6gwBZx1ccMMFyLgkVSupUmujYrFidEXuNRPq989";
const connection = new Connection("https://api.rpcpool.com/YOUR_TOKEN", {
  accountSync: {
    transport: "grpc",
    initialAccounts: [accountId],
  },
});

const account = await connection.getAccountInfo(new PublicKey(accountId));

await connection.close();
```

## What you get

The SDK is built to work as a web3.js-compatible `Connection`.

For account reads, it uses streaming account sync under the hood. For other web3.js methods, the connection still behaves like a normal web3.js connection.

Streaming-backed methods:

| Method                                                   | What it does                                                        |
| -------------------------------------------------------- | ------------------------------------------------------------------- |
| `getAccountInfo(publicKey, config?)`                     | Gets one account from the local stream-fed buffer.                  |
| `getAccountInfoAndContext(publicKey, config?)`           | Gets one account plus a context slot.                               |
| `getMultipleAccountsInfo(publicKeys, config?)`           | Gets many accounts from the local buffer.                           |
| `getMultipleAccountsInfoAndContext(publicKeys, config?)` | Gets many accounts plus one context slot.                           |
| `getParsedAccountInfo(publicKey, config?)`               | Gets one account and returns parsed data when parsing is supported. |
| `getMultipleParsedAccounts(publicKeys, config?)`         | Gets many parsed accounts.                                          |

SDK-only account stream controls:

| Method                       | What it does                                      |
| ---------------------------- | ------------------------------------------------- |
| `addAccounts(accountIds)`    | Adds accounts to the live stream.                 |
| `removeAccounts(accountIds)` | Stops tracking accounts.                          |
| `setAccounts(accountIds)`    | Replaces the full tracked account list.           |
| `close()`                    | Closes the stream and releases resources.         |
| `getLastTransportError()`    | Returns the latest stream error, if one happened. |

Account data helpers:

| Helper                                               | What it does                                                                    |
| ---------------------------------------------------- | ------------------------------------------------------------------------------- |
| `convertAccountData(data, fromEncoding, toEncoding)` | Converts account data between supported encodings.                              |
| `encodeAccount(account, encoding, options?)`         | Encodes account info into a Solana RPC-style account shape.                     |
| `parseJsonParsed(account, options?)`                 | Parses account data into `jsonParsed` form when supported.                      |
| `loadAccountEncodingWasm()`                          | Loads the WASM account encoder explicitly. Most users do not need to call this. |

## How reads work

The SDK fills its local buffer in two ways:

1. It does an initial JSON-RPC account fetch for accounts you track.
2. It keeps a stream open and applies live account updates as they arrive.

After that, `getAccountInfo` reads from the local buffer.

The stream can use gRPC or WebSocket. gRPC is for Node backends, workers, and scripts. WebSocket works in Node and browsers, so browser apps can use the same SDK with WebSocket streaming. See [Pick a transport](account-sync.md#pick-a-transport) and [Connection options](account-sync.md#connection-options).

The package and type exports are designed for both environments. TypeScript and bundlers use the package exports to pick the browser or backend build, so browser code gets the WebSocket-only connection type and Node code gets the Node connection type with both gRPC and WebSocket options.

This is why the main `endpoint` still matters. The SDK uses it for normal web3.js RPC calls and for the first account snapshot. The stream endpoint keeps the account state fresh after that.

```ts
const connection = new Connection(rpcEndpoint, {
  accountSync: {
    subscriptionEndpoint: streamEndpoint,
    transport: "grpc",
  },
});
```

## Quick start: Node with gRPC

Use gRPC in Node when you can. It is the best default for backend apps, workers, and scripts.

```ts
import { Connection, PublicKey } from "@triton-one/triton-sdk";

const endpoint = "https://api.rpcpool.com/YOUR_TOKEN";
const account = new PublicKey("ping6gwBZx1ccMMFyLgkVSupUmujYrFidEXuNRPq989");

const connection = new Connection(endpoint, {
  commitment: "confirmed",
  accountSync: {
    transport: "grpc",
    initialAccounts: [account],
    commitment: "confirmed",
    missTimeoutMs: 5_000,
  },
});

try {
  const accountInfo = await connection.getAccountInfo(account);

  if (accountInfo === null) {
    console.log("No account data is available yet.");
  } else {
    console.log("Lamports:", accountInfo.lamports);
    console.log("Owner:", accountInfo.owner.toBase58());
    console.log("Data bytes:", accountInfo.data.length);
  }
} finally {
  await connection.close();
}
```

## Quick start: WebSocket

Use WebSocket in browsers. You can also use it in Node.

```ts
import { Connection, PublicKey } from "@triton-one/triton-sdk";

const rpcEndpoint = "https://api.rpcpool.com/YOUR_TOKEN";
const wsEndpoint = "wss://api.rpcpool.com/YOUR_TOKEN";
const account = new PublicKey("ping6gwBZx1ccMMFyLgkVSupUmujYrFidEXuNRPq989");

const connection = new Connection(rpcEndpoint, {
  commitment: "confirmed",
  accountSync: {
    transport: "ws",
    subscriptionEndpoint: wsEndpoint,
    initialAccounts: [account],
    commitment: "confirmed",
  },
});

try {
  const accountInfo = await connection.getAccountInfo(account);
  console.log(accountInfo);
} finally {
  await connection.close();
}
```

Browsers only support WebSocket. Do not use `transport: "grpc"` in browser code.

## Browser example

In browser apps, use WebSocket. Your bundler should pick the browser build from the package automatically.

```ts
import { Connection, PublicKey } from "@triton-one/triton-sdk";

const connection = new Connection("https://api.rpcpool.com/YOUR_TOKEN", {
  accountSync: {
    transport: "ws",
    subscriptionEndpoint: "wss://api.rpcpool.com/YOUR_TOKEN",
    initialAccounts: [
      "So11111111111111111111111111111111111111112",
    ],
  },
});

const account = await connection.getParsedAccountInfo(
  new PublicKey("So11111111111111111111111111111111111111112"),
);

console.log(account.value);
```

If your app creates the connection inside a component, close it when the component is removed.

## Endpoint format

Use your Triton endpoint.

For gRPC:

```ts
const endpoint = "https://api.rpcpool.com/YOUR_TOKEN";
```

For WebSocket:

```ts
const wsEndpoint = "wss://api.rpcpool.com/YOUR_TOKEN";
```

You usually do not need to write those service paths yourself. Passing the host and token path is enough. For transport choice, see [Pick a transport](account-sync.md#pick-a-transport). For the full config shape, see [Connection options](account-sync.md#connection-options).

## Pick a transport

Use this simple rule:

| App type     | Use                 |
| ------------ | ------------------- |
| Node backend | `transport: "grpc"` |
| Node worker  | `transport: "grpc"` |
| Node script  | `transport: "grpc"` |
| Browser app  | `transport: "ws"`   |

If your network blocks gRPC, use WebSocket.

## Method reference

These are the main method signatures.

```ts
new Connection(
  endpoint: string,
  commitmentOrConfig?:
    | Commitment
    | NodeAccountSyncConnectionConfig
    | BrowserAccountSyncConnectionConfig,
);
```

```ts
getAccountInfo(
  publicKey: PublicKey,
  commitmentOrConfig?: Commitment | GetAccountInfoConfig,
): Promise<AccountInfo<Buffer> | null>;
```

```ts
getAccountInfoAndContext(
  publicKey: PublicKey,
  commitmentOrConfig?: Commitment | GetAccountInfoConfig,
): Promise<RpcResponseAndContext<AccountInfo<Buffer> | null>>;
```

```ts
getMultipleAccountsInfo(
  publicKeys: PublicKey[],
  commitmentOrConfig?: Commitment | GetMultipleAccountsConfig,
): Promise<(AccountInfo<Buffer> | null)[]>;
```

```ts
getMultipleAccountsInfoAndContext(
  publicKeys: PublicKey[],
  commitmentOrConfig?: Commitment | GetMultipleAccountsConfig,
): Promise<RpcResponseAndContext<(AccountInfo<Buffer> | null)[]>>;
```

```ts
getParsedAccountInfo(
  publicKey: PublicKey,
  commitmentOrConfig?: Commitment | GetAccountInfoConfig,
): Promise<RpcResponseAndContext<AccountInfo<Buffer | ParsedAccountData> | null>>;
```

```ts
getMultipleParsedAccounts(
  publicKeys: PublicKey[],
  rawConfig?: GetMultipleAccountsConfig,
): Promise<RpcResponseAndContext<(AccountInfo<Buffer | ParsedAccountData> | null)[]>>;
```

```ts
addAccounts(accountIds: ReadonlyArray<string | PublicKey>): Promise<void>;
removeAccounts(accountIds: ReadonlyArray<string | PublicKey>): Promise<void>;
setAccounts(accountIds: ReadonlyArray<string | PublicKey>): Promise<void>;
close(): Promise<void>;
getLastTransportError(): Error | null;
```

In Node, the config type is `NodeAccountSyncConnectionConfig`. In browser code, the config type is `BrowserAccountSyncConnectionConfig`.

## Read options

The read methods accept the same style as web3.js:

```ts
await connection.getAccountInfo(account, "confirmed");
```

or:

```ts
await connection.getAccountInfo(account, {
  commitment: "confirmed",
  dataSlice: { offset: 8, length: 32 },
  minContextSlot: 123456,
});
```

Supported read options:

| Option           | Meaning                                                                   |
| ---------------- | ------------------------------------------------------------------------- |
| `commitment`     | Reads from the buffer for `"processed"`, `"confirmed"`, or `"finalized"`. |
| `dataSlice`      | Returns only part of the account data.                                    |
| `minContextSlot` | Waits for a buffered account update at or above this slot.                |

Important: `minContextSlot` is checked against the SDK local buffer. If the stream does not deliver an update at or above that slot before `missTimeoutMs`, the SDK returns `null`.

## Read one account

```ts
import { Connection, PublicKey } from "@triton-one/triton-sdk";

const connection = new Connection("https://api.rpcpool.com/YOUR_TOKEN", {
  accountSync: {
    transport: "grpc",
    initialAccounts: [
      "ping6gwBZx1ccMMFyLgkVSupUmujYrFidEXuNRPq989",
    ],
  },
});

const publicKey = new PublicKey("ping6gwBZx1ccMMFyLgkVSupUmujYrFidEXuNRPq989");
const accountInfo = await connection.getAccountInfo(publicKey);

if (accountInfo) {
  console.log(accountInfo.lamports);
  console.log(accountInfo.owner.toBase58());
}

await connection.close();
```

## Read one account with context

Use this when you need to know which slot the data came from.

```ts
const response = await connection.getAccountInfoAndContext(publicKey, {
  commitment: "confirmed",
});

console.log("slot:", response.context.slot);
console.log("account:", response.value);
```

If `response.value` is `null`, the context slot is `0`.

## Read many accounts

The result order matches the input order.

```ts
const accounts = await connection.getMultipleAccountsInfo([
  new PublicKey("ping6gwBZx1ccMMFyLgkVSupUmujYrFidEXuNRPq989"),
  new PublicKey("So11111111111111111111111111111111111111112"),
]);

for (const accountInfo of accounts) {
  if (accountInfo === null) {
    console.log("missing account");
  } else {
    console.log(accountInfo.lamports);
  }
}
```

## Read many accounts with context

```ts
const response = await connection.getMultipleAccountsInfoAndContext(
  [
    new PublicKey("ping6gwBZx1ccMMFyLgkVSupUmujYrFidEXuNRPq989"),
    new PublicKey("So11111111111111111111111111111111111111112"),
  ],
  { commitment: "confirmed" },
);

console.log("slot:", response.context.slot);
console.log("accounts:", response.value);
```

For multiple-account reads, the context slot is the lowest slot among the returned non-null accounts. If every account is `null`, the context slot is `0`.

## Read parsed accounts

Use parsed reads when you want web3.js-style parsed account data.

```ts
const response = await connection.getParsedAccountInfo(
  new PublicKey("So11111111111111111111111111111111111111112"),
  { commitment: "confirmed" },
);

console.log(response.context.slot);
console.log(response.value?.data);
```

Many parsed accounts:

```ts
const response = await connection.getMultipleParsedAccounts(
  [
    new PublicKey("So11111111111111111111111111111111111111112"),
    new PublicKey("TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"),
  ],
  { commitment: "confirmed" },
);

console.log(response.value);
```

If a program parser is not supported, the SDK falls back to raw base64 account data. This matches normal RPC behaviour.

## Use `dataSlice`

Use `dataSlice` when you only need a small part of account data.

```ts
const accountInfo = await connection.getAccountInfo(publicKey, {
  commitment: "confirmed",
  dataSlice: {
    offset: 8,
    length: 32,
  },
});

console.log(accountInfo?.data);
```

`dataSlice` changes `accountInfo.data`. It does not change `accountInfo.space`. `space` is the full account data size.

## Use `minContextSlot`

Use `minContextSlot` when you only want data from a slot at or above a known slot.

```ts
const accountInfo = await connection.getAccountInfo(publicKey, {
  commitment: "confirmed",
  minContextSlot: 250_000_000,
});

if (accountInfo === null) {
  console.log("The local buffer did not reach that slot in time.");
}
```

This SDK does not use `minContextSlot` exactly like direct web3.js RPC. Direct RPC asks a node to serve a read at a given context. This SDK checks the latest stream update in its local buffer.

## Manage the tracked account set

The tracked account set is the list of accounts the SDK asks the stream to send updates for.

You can start with `initialAccounts`, then change the tracked set while your app is running.

```ts
const connection = new Connection(endpoint, {
  accountSync: {
    transport: "grpc",
    initialAccounts: [
      "ping6gwBZx1ccMMFyLgkVSupUmujYrFidEXuNRPq989",
    ],
  },
});
```

You can pass account ids as strings or `PublicKey` values.

### Add accounts

Use `addAccounts` when you want to keep the current tracked accounts and add more.

```ts
await connection.addAccounts([
  "ping6gwBZx1ccMMFyLgkVSupUmujYrFidEXuNRPq989",
  new PublicKey("So11111111111111111111111111111111111111112"),
]);
```

After this call, those accounts are part of the live stream.

### Remove accounts

Use `removeAccounts` when you no longer want live updates for some accounts.

```ts
await connection.removeAccounts([
  "ping6gwBZx1ccMMFyLgkVSupUmujYrFidEXuNRPq989",
]);
```

This stops tracking those accounts on the stream. It does not erase any account data already stored in the local buffer.

### Replace all accounts

Use `setAccounts` when you want to replace the full tracked set.

```ts
await connection.setAccounts([
  new PublicKey("So11111111111111111111111111111111111111112"),
]);
```

After this call, the stream tracks only the accounts in the new list.

### Tracked set notes

* Duplicate accounts are normalised to one tracked account.
* Bad public keys throw before the stream is updated.
* These methods update the stream used by the connection's default `accountSync.commitment`.
* If `autoSubscribeOnMiss` is `true`, reading an untracked account can add it to the stream automatically.

## Auto-subscribe on miss

By default, if you call `getAccountInfo` for an account that is not already tracked, the SDK subscribes to it and waits up to `missTimeoutMs`.

```ts
const connection = new Connection(endpoint, {
  accountSync: {
    transport: "grpc",
    autoSubscribeOnMiss: true,
    missTimeoutMs: 10_000,
  },
});
```

If you want reads to return `null` immediately for untracked accounts, turn it off:

```ts
const connection = new Connection(endpoint, {
  accountSync: {
    transport: "grpc",
    autoSubscribeOnMiss: false,
  },
});
```

## Close the connection

Call `close()` when your script, test, or app page is done with the connection.

```ts
await connection.close();
```

For a long-running server, call it during shutdown.

## Check stream errors

If account reads keep returning `null`, check the transport error.

```ts
const error = connection.getLastTransportError();

if (error) {
  console.error("stream error:", error.message);
}
```

## Use normal web3.js methods

The SDK connection is built to be a web3.js drop-in replacement.

```ts
const blockHeight = await connection.getBlockHeight();
const balance = await connection.getBalance(publicKey);
const accountInfo = await connection.getAccountInfo(publicKey);
```

The account read methods above are stream-backed. Other web3.js methods use the normal RPC connection behaviour.

## Account data encoding helpers

The SDK includes helpers for account data encoding.

```ts
import { convertAccountData } from "@triton-one/triton-sdk";

const base58Data = await convertAccountData(
  "base64-account-data-here",
  "base64",
  "base58",
);

console.log(base58Data);
```

Supported encodings:

* `binary`
* `base58`
* `base64`
* `base64+zstd`
* `jsonParsed`

Most users do not need these helpers. Use them when you need raw account data in a specific Solana RPC encoding.

## Why `null` happens

`getAccountInfo` can return `null`. This is normal.

Common reasons:

* The account does not exist.
* The account is not tracked and `autoSubscribeOnMiss` is `false`.
* The stream has not delivered the account yet.
* The account did not reach `minContextSlot` before `missTimeoutMs`.
* The endpoint, token, or transport is wrong.
* You are asking for a different commitment than the stream has delivered.

Common fixes:

```ts
const connection = new Connection(endpoint, {
  accountSync: {
    transport: "grpc",
    initialAccounts: [publicKey],
    autoSubscribeOnMiss: true,
    missTimeoutMs: 10_000,
  },
});
```

Then retry the read:

```ts
for (let attempt = 0; attempt < 10; attempt += 1) {
  const accountInfo = await connection.getAccountInfo(publicKey);

  if (accountInfo) {
    console.log(accountInfo);
    break;
  }

  await new Promise((resolve) => setTimeout(resolve, 500));
}
```

## Common mistakes

### Using gRPC in the browser

Browsers cannot use `@grpc/grpc-js`.

Use WebSocket in browser apps:

```ts
accountSync: {
  transport: "ws",
}
```

### Forgetting to close in scripts

If a script does not exit, close the connection:

```ts
await connection.close();
```

### Passing a bad public key

All account ids must be valid Solana public keys.

```ts
const publicKey = new PublicKey("So11111111111111111111111111111111111111112");
```

### Expecting old historical state

The SDK stores the latest streamed state. It is not a historical database.

If you need old account state from a past slot, use a service built for historical reads.

## Full example

```ts
import { Connection, PublicKey } from "@triton-one/triton-sdk";

const endpoint = "https://api.rpcpool.com/YOUR_TOKEN";
const publicKey = new PublicKey("ping6gwBZx1ccMMFyLgkVSupUmujYrFidEXuNRPq989");

const connection = new Connection(endpoint, {
  commitment: "confirmed",
  accountSync: {
    transport: "grpc",
    initialAccounts: [publicKey],
    commitment: "confirmed",
    autoSubscribeOnMiss: true,
    missTimeoutMs: 5_000,
  },
});

try {
  const accountInfo = await connection.getAccountInfo(publicKey, {
    commitment: "confirmed",
  });

  if (!accountInfo) {
    throw new Error("account was not available");
  }

  console.log({
    lamports: accountInfo.lamports,
    owner: accountInfo.owner.toBase58(),
    executable: accountInfo.executable,
    rentEpoch: accountInfo.rentEpoch,
    dataLength: accountInfo.data.length,
    space: accountInfo.space,
  });
} finally {
  await connection.close();
}
```

## Pricing

Account Sync is billed on bandwidth only, `$0.08 / GB` of streamed data, with no per-call charge.

***

<i class="fa-life-ring">:life-ring:</i> Contact support by clicking the chat icon in your [customer dashboard](https://customers.triton.one)\
<i class="fa-briefcase">:briefcase:</i> Sales questions? [Contact us](https://triton.one/contact)\
<i class="fa-sparkles">:sparkles:</i> AI agent? Read [llms.txt](https://docs.triton.one/llms.txt)\
<i class="fa-rss">:rss:</i> Follow updates: [Blog](https://blog.triton.one) · [X](https://x.com/triton_one) · [YouTube](https://www.youtube.com/@triton_one_ltd) · [Telegram](https://t.me/tritonone) · [GitHub](https://github.com/rpcpool)
