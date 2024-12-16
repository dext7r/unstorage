---
icon: material-symbols:cached-rounded
---

# LRU 缓存

> 使用 LRU 缓存在内存中保持缓存数据。

## 用法

使用 [LRU 缓存](https://www.npmjs.com/package/lru-cache) 在内存中保持缓存数据。

请参阅 [`lru-cache`](https://www.npmjs.com/package/lru-cache) 以获取支持的选项。

默认情况下，[`max`](https://www.npmjs.com/package/lru-cache#max) 设置为 `1000` 项。

[`sizeCalculation`](https://www.npmjs.com/package/lru-cache#sizecalculation) 选项的默认行为是基于键和值的缓冲区大小实现的。

```js
import { createStorage } from "unstorage";
import lruCacheDriver from "unstorage/drivers/lru-cache";

const storage = createStorage({
  driver: lruCacheDriver(),
});