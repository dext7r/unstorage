---
icon: devicon-plain:cloudflareworkers
---

# Cloudflare

> 在 Cloudflare KV 或 R2 存储中存储数据。

## CloudFlare KV（绑定）

> 在 Cloudflare KV 中存储数据并通过 Worker 绑定进行访问。

### 使用方法

::read-more{to="https://developers.cloudflare.com/workers/runtime-apis/kv"}
了解更多关于 Cloudflare KV 的信息。
::

**注意：** 此驱动程序仅在 Cloudflare Worker 环境中有效，对于其他环境，请使用 `cloudflare-kv-http`。

您需要创建并分配一个 KV。有关更多信息，请参见 [KV 绑定](https://developers.cloudflare.com/workers/runtime-apis/kv#kv-bindings)。

```js
import { createStorage } from "unstorage";
import cloudflareKVBindingDriver from "unstorage/drivers/cloudflare-kv-binding";

// 使用从 globalThis 中选择的绑定名称
const storage = createStorage({
  driver: cloudflareKVBindingDriver({ binding: "STORAGE" }),
});

// 直接设置绑定
const storage = createStorage({
  driver: cloudflareKVBindingDriver({ binding: globalThis.STORAGE }),
});

// 使用 Durable Objects 和模块语法从 Workers 中获取
const storage = createStorage({
  driver: cloudflareKVBindingDriver({ binding: this.env.STORAGE }),
});

// 在 Cloudflare Workers 之外使用（如 Node.js）
// 使用 cloudflare-kv-http
```

**选项：**

- `binding`：KV 绑定或命名空间的名称。默认值为 `STORAGE`。
- `base`：为所有存储的键添加前缀。

## Cloudflare KV（http）

> 使用 Cloudflare API v4 在 Cloudflare KV 中存储数据。

### 使用方法

::read-more{to="https://developers.cloudflare.com/api/operations/workers-kv-namespace-list-namespaces"}
了解更多关于 Cloudflare KV API 的信息。
::

您需要创建一个 KV 命名空间。有关更多信息，请参见 [KV 绑定](https://developers.cloudflare.com/workers/runtime-apis/kv#kv-bindings)。

**注意：** 此驱动程序使用本地 fetch，通用有效！对于 Cloudflare Worker 环境中的直接使用，请使用 `cloudflare-kv-binding` 驱动程序以获取最佳性能！

```js
import { createStorage } from "unstorage";
import cloudflareKVHTTPDriver from "unstorage/drivers/cloudflare-kv-http";

// 使用 `apiToken`
const storage = createStorage({
  driver: cloudflareKVHTTPDriver({
    accountId: "my-account-id",
    namespaceId: "my-kv-namespace-id",
    apiToken: "supersecret-api-token",
  }),
});

// 使用 `email` 和 `apiKey`
const storage = createStorage({
  driver: cloudflareKVHTTPDriver({
    accountId: "my-account-id",
    namespaceId: "my-kv-namespace-id",
    email: "me@example.com",
    apiKey: "my-api-key",
  }),
});

// 使用 `userServiceKey`
const storage = createStorage({
  driver: cloudflareKVHTTPDriver({
    accountId: "my-account-id",
    namespaceId: "my-kv-namespace-id",
    userServiceKey: "v1.0-my-service-key",
  }),
});
```

**选项：**

- `accountId`：Cloudflare 账户 ID。
- `namespaceId`：要访问的 KV 命名空间的 ID。**注意：** 确保使用命名空间的 ID，而不是 Worker 环境中使用的名称或绑定。
- `apiToken`：从 [用户个人资料“API Tokens”页面](https://dash.cloudflare.com/profile/api-tokens) 生成的 API Token。
- `email`：与您的账户关联的电子邮件地址。可以与 `apiKey` 一起使用以替代 `apiToken` 进行身份验证。
- `apiKey`：在 Cloudflare 控制台的“我的账户”页面生成的 API 密钥。可以与 `email` 一起使用以替代 `apiToken` 进行身份验证。
- `userServiceKey`：一种特殊的 Cloudflare API 密钥，仅用于有限的端点。始终以 "v1.0-" 开头，长度可能不同。可以替代 `apiToken` 或 `apiKey` 和 `email` 进行身份验证。
- `apiURL`：自定义 API URL。默认值为 `https://api.cloudflare.com`。
- `base`：为所有存储的键添加前缀。

**事务选项：**

- `ttl`：支持 `setItem(key, value, { ttl: number /* seconds min 60 */ })`。

**支持的方法：**

- `getItem`：映射到 [读取键值对](https://api.cloudflare.com/#workers-kv-namespace-read-key-value-pair) `GET accounts/:account_identifier/storage/kv/namespaces/:namespace_identifier/values/:key_name`。
- `hasItem`：映射到 [读取键值对](https://api.cloudflare.com/#workers-kv-namespace-read-key-value-pair) `GET accounts/:account_identifier/storage/kv/namespaces/:namespace_identifier/values/:key_name`。如果 `<parsed response body>.success` 为 `true`，则返回 `true`。
- `setItem`：映射到 [写入键值对](https://api.cloudflare.com/#workers-kv-namespace-write-key-value-pair) `PUT accounts/:account_identifier/storage/kv/namespaces/:namespace_identifier/values/:key_name`。
- `removeItem`：映射到 [删除键值对](https://api.cloudflare.com/#workers-kv-namespace-delete-key-value-pair) `DELETE accounts/:account_identifier/storage/kv/namespaces/:namespace_identifier/values/:key_name`。
- `getKeys`：映射到 [列出命名空间的键](https://api.cloudflare.com/#workers-kv-namespace-list-a-namespace-s-keys) `GET accounts/:account_identifier/storage/kv/namespaces/:namespace_identifier/keys`。
- `clear`：映射到 [删除键值对](https://api.cloudflare.com/#workers-kv-namespace-delete-multiple-key-value-pairs) `DELETE accounts/:account_identifier/storage/kv/namespaces/:namespace_identifier/bulk`。

## CloudFlare R2（绑定）

> 在 Cloudflare R2 存储桶中存储数据并通过 Worker 绑定进行访问。

::warning
这是一个实验性驱动程序！此驱动程序仅在 Cloudflare Worker 环境中有效，无法在其他运行时环境（如 Node.js）中使用（r2-http 驱动程序即将推出）。
::

::read-more{to="https://developers.cloudflare.com/r2/api/workers/workers-api-reference/"}
了解更多关于 Cloudflare R2 存储桶的信息。
::

您需要创建并分配一个 R2 存储桶。有关更多信息，请参见 [R2 绑定](https://developers.cloudflare.com/r2/api/workers/workers-api-reference/#create-a-binding)。

```js
import { createStorage } from "unstorage";
import cloudflareR2BindingDriver from "unstorage/drivers/cloudflare-r2-binding";

// 使用从 globalThis 中选择的绑定名称
const storage = createStorage({
  driver: cloudflareR2BindingDriver({ binding: "BUCKET" }),
});

// 直接设置绑定
const storage = createStorage({
  driver: cloudflareR2BindingDriver({ binding: globalThis.BUCKET }),
});

// 使用 Durable Objects 和模块语法从 Workers 中获取
const storage = createStorage({
  driver: cloudflareR2BindingDriver({ binding: this.env.BUCKET }),
});
```

**选项：**

- `binding`：存储桶绑定或名称。默认值为 `BUCKET`。
- `base`：为所有键添加前缀。

**事务选项：**

- `getItemRaw(key, { type: "..." })`
  - `type: "object"`：返回 [R2 对象主体](https://developers.cloudflare.com/r2/api/workers/workers-api-reference/#r2objectbody-definition)。
  - `type: "stream"`：返回主体流。
  - `type: "blob"`：返回一个 `Blob`。
  - `type: "bytes"`：返回一个 `Uint8Array`。
  - `type: "arrayBuffer"`：返回一个 `ArrayBuffer`（默认）。