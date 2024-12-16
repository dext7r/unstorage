---
icon: simple-icons:deno
---

# Deno KV

> 在 Deno KV 中存储数据

::note{to="https://deno.com/kv"}
了解更多关于 Deno KV 的信息。
::

## 用法 (Deno)

::important
`deno-kv` 驱动程序需要 [Deno 部署](https://docs.deno.com/deploy/kv/manual/on_deploy/) 或带有 `--unstable-kv` CLI 标志的 [Deno 运行时](https://docs.deno.com/runtime/)。有关其他运行时的信息，请参见 [Node.js](#usage-nodejs) 部分。
::

::note
该驱动程序会自动将 Unstorage 键映射到 Deno。例如，`"test:key"` 键将映射为 `["test", "key"]`，反之亦然。
::

```js
import { createStorage } from "unstorage";
import denoKVdriver from "unstorage/drivers/deno-kv";

const storage = createStorage({
  driver: denoKVdriver({
    // path: ":memory:",
    // base: "",
  }),
});
```

**选项：**

- `path`：（可选）文件系统路径，用于存储数据库，否则将根据脚本的当前工作目录由 Deno 创建一个。您可以传入 `:memory:` 进行测试。
- `base`：（可选）添加到所有操作的前缀键。
- `openKV`：（高级）自定义方法，返回 Deno KV 实例。

## 用法 (Node.js)

Deno 提供了 [`@deno/kv`](https://www.npmjs.com/package/@deno/kv) npm 包，这是一个优化用于 Node.js 的 Deno KV 客户端库。

- 访问 [Deno Deploy](https://deno.com/deploy) 远程数据库（或任何实现开放
  [KV Connect](https://github.com/denoland/denokv/blob/main/proto/kv-connect.md)
  协议的端点），支持 Node 18+。
- 创建基于
  [SQLite](https://www.sqlite.org/index.html) 的本地 KV 数据库，使用与 Deno 本身创建的数据库兼容的优化原生
  [NAPI](https://nodejs.org/docs/latest-v18.x/api/n-api.html) 包。
- 创建基于 SQLite 内存文件或轻量级的仅 JS 实现的临时内存 KV 实例，用于测试。

安装 `@deno/kv` 作为对等依赖：

:pm-install{name="@deno/kv"}

```js
import { createStorage } from "unstorage";
import denoKVNodedriver from "unstorage/drivers/deno-kv-node";

const storage = createStorage({
  driver: denoKVNodedriver({
    // path: ":memory:",
    // base: "",
  }),
});
```

**选项：**

- `path`：（与 `deno-kv` 相同）
- `base`：（与 `deno-kv` 相同）
- `openKvOptions`：查看 [文档](https://www.npmjs.com/package/@deno/kv#api) 以获取可用选项。