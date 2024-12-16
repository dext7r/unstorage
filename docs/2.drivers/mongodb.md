---
icon: teenyicons:mongodb-outline
---

# MongoDB

> 使用 Node.js MongoDB 包在 MongoDB 中存储数据。

## 用法

::read-more{to="https://www.mongodb.com/"}
了解更多关于 MongoDB 的信息。
::

此驱动程序将 KV 信息存储在 MongoDB 集合中，每个键值对有一个单独的文档。

要使用它，您需要在项目中安装 `mongodb`：

:pm-install{name="mongodb"}

用法：

```js
import { createStorage } from "unstorage";
import mongodbDriver from "unstorage/drivers/mongodb";

const storage = createStorage({
  driver: mongodbDriver({
    connectionString: "CONNECTION_STRING",
    databaseName: "test",
    collectionName: "test",
  }),
});
```

**认证：**

该驱动程序支持以下认证方法：

- **`connectionString`**：MongoDB 连接字符串。这是进行身份验证的唯一方法。

**选项：**

- **`connectionString`**（必需）：用于连接到 MongoDB 数据库的连接字符串。应采用格式 `mongodb://<username>:<password>@<host>:<port>/<database>`。
- `databaseName`：要使用的数据库名称。默认为 `unstorage`。
- `collectionName`：要使用的集合名称。默认为 `unstorage`。