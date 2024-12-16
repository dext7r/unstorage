---
icon: ic:baseline-http
---

# HTTP

> 使用远程 HTTP/HTTPS 端点作为数据存储。

## 用法

::note
支持内置的 [http server](/guide/http-server) 方法。
::

该驱动为每个键实现元数据，包括通过发起 `HEAD` 请求获取的 `mtime`（最后修改时间）和 `status`（HTTP 头部）。

```js
import { createStorage } from "unstorage";
import httpDriver from "unstorage/drivers/http";

const storage = createStorage({
  driver: httpDriver({ base: "http://cdn.com" }),
});
```

**选项:**

- `base`: URL 基础路径 (**必填**)
- `headers`: 所有请求的自定义头部

**支持的 HTTP 方法:**

- `getItem`: 映射到 HTTP `GET`。如果响应正常，则返回反序列化的值
- `hasItem`: 映射到 HTTP `HEAD`。如果响应正常（200），则返回 `true`
- `getMeta`: 映射到 HTTP `HEAD`（头部: `last-modified` => `mtime`, `x-ttl` => `ttl`）
- `setItem`: 映射到 HTTP `PUT`。使用请求体发送序列化值（`ttl` 选项将作为 `x-ttl` 头部发送）。
- `removeItem`: 映射到 `DELETE`
- `clear`: 不支持

**事务选项:**

- `headers`: 在每个操作中发送的自定义头部（`getItem`、`setItem` 等）
- `ttl`: 支持驱动的自定义 `ttl`（以秒为单位）。将映射到 `x-ttl` HTTP 头部。