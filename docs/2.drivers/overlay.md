---
icon: carbon:overlay
---

# Overlay

这是一个特殊的驱动程序，创建一个多层叠加驱动程序。

所有的写操作都发生在最上面的层级，而值则从所有层级中读取。

当移除一个键时，最上层会内部设置一个特殊值 `__OVERLAY_REMOVED__`。

在下面的示例中，我们在文件系统上创建一个内存叠加，设置新键时不会对磁盘进行实际写入。

```js
import { createStorage } from "unstorage";
import overlay from "unstorage/drivers/overlay";
import memory from "unstorage/drivers/memory";
import fs from "unstorage/drivers/fs";

const storage = createStorage({
  driver: overlay({
    layers: [memory(), fs({ base: "./data" })],
  }),
});