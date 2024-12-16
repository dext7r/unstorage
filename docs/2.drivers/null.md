---
icon: bi:trash3-fill
---

# Null

> 丢弃所有数据。

::warning
此驱动程序不会存储任何数据。它将丢弃写入的数据，并始终返回 null，类似于 [`/dev/null`](https://en.wikipedia.org/wiki/Null_device)
::

```js
import { createStorage } from "unstorage";
import nullDriver from "unstorage/drivers/null";

const storage = createStorage({
  driver: nullDriver(),
});