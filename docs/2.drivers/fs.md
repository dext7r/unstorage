---
icon: ph:file-light
---

# 文件系统 (Node.js)

> 使用 Node.js API 将数据存储在文件系统中。

## 用法

使用目录结构将数据映射到真实的文件系统，以支持嵌套键。支持使用 [chokidar](https://github.com/paulmillr/chokidar) 进行监视。

这个驱动程序为每个键实现了元数据，包括 `mtime`（最后修改时间）、`atime`（最后访问时间）和 `size`（文件大小），使用 `fs.stat`。

```js
import { createStorage } from "unstorage";
import fsDriver from "unstorage/drivers/fs";

const storage = createStorage({
  driver: fsDriver({ base: "./tmp" }),
});
```

**选项:**

- `base`: 基础目录，隔离对该目录的操作
- `ignore`: 监视时忽略的模式 <!-- 和键列表 -->
- `watchOptions`: 额外的 [chokidar](https://github.com/paulmillr/chokidar) 选项。

## Node.js 文件系统 (Lite)

这个驱动程序使用纯 Node.js API，没有额外的依赖。

```js
import { createStorage } from "unstorage";
import fsLiteDriver from "unstorage/drivers/fs-lite";

const storage = createStorage({
  driver: fsLiteDriver({ base: "./tmp" }),
});
```

**选项:**

- `base`: 基础目录，隔离对该目录的操作
- `ignore`: 可选的回调函数 `(path: string) => boolean`