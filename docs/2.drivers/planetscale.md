---
icon: simple-icons:planetscale
---

# PlanetScale

> 通过 PlanetScale 在 MySQL 数据库中存储数据。

## 使用方法

::read-more{to="https://docs.microsoft.com/en-us/azure/key-vault/secrets/about-secrets"}
了解有关 PlanetScale 的更多信息。
::

该驱动程序在 Planetscale 数据库中存储 KV 信息，列包含 `id`、`value`、`created_at` 和 `updated_at`。

使用前，您需要在项目中安装 `@planetscale/database`：

```json
{
  "dependencies": {
    "@planetscale/database": "^1.5.0"
  }
}
```

然后，您可以通过在 Planetscale 数据库中运行以下查询来创建一个表以存储数据，其中 `<storage>` 是您希望使用的表的名称：

```
create table <storage> (
 id varchar(255) not null primary key,
 value longtext,
 created_at timestamp default current_timestamp,
 updated_at timestamp default current_timestamp on update current_timestamp
);
```

之后，您可以像这样配置驱动程序：

```js
import { createStorage } from "unstorage";
import planetscaleDriver from "unstorage/drivers/planetscale";

const storage = createStorage({
  driver: planetscaleDriver({
    // 这绝对不应内联在代码中，而应通过运行时配置或环境变量加载，具体取决于您的框架/项目。
    url: "mysql://xxxxxxxxx:************@xxxxxxxxxx.us-east-3.psdb.cloud/my-database?sslaccept=strict",
    // table: 'storage'
  }),
});
```

**选项：**

- **`url`**（必需）：您可以在 [Planetscale 控制台](https://planetscale.com/docs/tutorials/connect-nodejs-app) 中找到您的 URL。
- `table`：要读取的表的名称。默认为 `storage`。
- `boostCache`：是否启用缓存查询：请参阅 [文档](https://planetscale.com/docs/concepts/query-caching-with-planetscale-boost#using-cached-queries-in-your-application)。