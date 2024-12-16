---
icon: gg:vercel
---

# Vercel

## Vercel KV

> 在 Vercel KV 存储中存储数据。

::read-more{to="https://vercel.com/docs/storage/vercel-kv"}
了解更多关于 Vercel KV 的信息。
::

> [!NOTE]
> 请检查 [Vercel KV 限制](https://vercel.com/docs/storage/vercel-kv/limits)。

```js
import { createStorage } from "unstorage";
import vercelKVDriver from "unstorage/drivers/vercel-kv";

const storage = createStorage({
  driver: vercelKVDriver({
    // url: "https://<your-project-slug>.kv.vercel-storage.com", // KV_REST_API_URL
    // token: "<your secret token>", // KV_REST_API_TOKEN
    // base: "test",
    // env: "KV",
    // ttl: 60, // 秒
  }),
});
```

要使用，您需要在项目中安装 `@vercel/kv` 依赖：

```json
{
  "dependencies": {
    "@vercel/kv": "latest"
  }
}
```

**注意：** 对于驱动程序选项类型支持，您可能还需要安装 `@upstash/redis` 开发依赖。

**选项:**

- `url`: 用于连接到您的 Vercel KV 存储的 REST API URL。默认值为 `KV_REST_API_URL`。
- `token`: 用于连接到您的 Vercel KV 存储的 REST API 令牌。默认值为 `KV_REST_API_TOKEN`。
- `base`: [可选] 用于所有键的前缀。可用于命名空间。
- `env`: [可选] 自定义环境变量前缀的标志（默认值为 `KV`）。设置为 `false` 以禁用 `url` 和 `token` 选项的环境推断。

有关所有可用选项，请参见 [@upstash/redis](https://docs.upstash.com/redis/sdks/javascriptsdk/advanced)。

## Vercel Blob

> 在 Vercel Blob 存储中存储数据。

::read-more{to="https://vercel.com/docs/storage/vercel-blob"}
了解更多关于 Vercel Blob 的信息。
::

::warning
当前 Vercel Blob 存储所有数据均为公共访问。
::

要使用，您需要在项目中安装 [`@vercel/blob`](https://www.npmjs.com/package/@vercel/blob) 依赖：

:pm-install{name="@vercel/blob"}

```js
import { createStorage } from "unstorage";
import vercelBlobDriver from "unstorage/drivers/vercel-blob";

const storage = createStorage({
  driver: vercelBlobDriver({
    access: "public", // 必需！请注意，存储的数据是公开可访问的。
    // token: "<your secret token>", // 或设置 BLOB_READ_WRITE_TOKEN
    // base: "unstorage",
    // envPrefix: "BLOB",
  }),
});
```

**选项:**

- `access`: blob 是否应公开可访问。（必需，必须为 `public`）
- `base`: 要预先添加到所有键的前缀。可用于命名空间。
- `token`: 用于连接到您的 Vercel Blob 存储的 REST API 令牌。如果未提供，将从环境变量 `BLOB_READ_WRITE_TOKEN` 中读取。
- `envPrefix`: 用于令牌环境变量名称的前缀。默认值为 `BLOB`（环境名称 = `BLOB_READ_WRITE_TOKEN`）。