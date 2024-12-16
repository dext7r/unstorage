---
icon: bi:memory
---

# 内存

> 将数据保存在内存中。

使用 [Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) 将数据保存在内存中。（默认存储）

::note
默认情况下，它被挂载在顶层，因此不太可能需要再次挂载它。
::

```js
import { createStorage } from "unstorage";
import memoryDriver from "unstorage/drivers/memory";

const storage = createStorage({
  driver: memoryDriver(),
});