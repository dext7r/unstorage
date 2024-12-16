---
icon: ph:database
---

# SQL 数据库

> 在任何 SQL 数据库中存储数据。

## 使用方法

该驱动程序使用 [db0](https://db0.unjs.io) 在任何 SQL 数据库中存储 KV 数据。

::warning
数据库驱动程序处于实验阶段，请在 [这里](https://github.com/unjs/unstorage/issues/400) 报告任何问题。
::

要使用此驱动程序，您需要在项目中安装 `db0`：

:pm-install{name="db0"}

选择并配置适合您数据库的连接器。

::important{to="https://db0.unjs.io/connectors"}
在 `db0` 文档中了解有关配置连接器的更多信息。
::

然后，您可以像这样配置驱动程序：

```js
import { createDatabase } from "db0";
import { createStorage } from "unstorage";
import dbDriver from "unstorage/drivers/db0";
import sqlite from "db0/connectors/better-sqlite3";

// 了解更多信息: https://db0.unjs.io
const database = createDatabase(
  sqlite({
    /* db0 连接器选项 */
  })
);

const storage = createStorage({
  driver: dbDriver({
    database,
    table: "custom_table_name", // 默认是 "unstorage"
  }),
});
```

::tip
数据库表会自动创建，无需额外设置！ <br>
在第一次操作之前，驱动程序确保存在包含 `id`、`value`、`blob`、`created_at` 和 `updated_at` 列的表。
::

**选项:**

- **`database`** (必填): 一个 `db0` 数据库实例。
- `table`: 要使用的表名。默认为 `unstorage`。