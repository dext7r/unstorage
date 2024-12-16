---
icon: simple-icons:redis
---

# Redis

> 将数据存储在Redis中。

## 使用方法

::read-more{to="https://redis.com"}
了解更多关于Redis的信息。
::

::note
Unstorage使用[`ioredis`](https://github.com/luin/ioredis)作为内部连接Redis的组件。
::

要使用它，您需要在项目中安装`ioredis`：

:pm-install{name="ioredis"}

使用单个Redis实例：

```js
import { createStorage } from "unstorage";
import redisDriver from "unstorage/drivers/redis";

const storage = createStorage({
  driver: redisDriver({
    base: "unstorage",
    host: 'HOSTNAME',
    tls: true as any,
    port: 6380,
    password: 'REDIS_PASSWORD'
  }),
});
```

使用Redis集群（例如AWS ElastiCache或Azure Redis Cache）：

⚠️ 如果您连接到集群，您必须使用`hastags`作为前缀，以避免Redis错误`CROSSSLOT Keys in request don't hash to the same slot`。这意味着，前缀必须用花括号包围，这会强制将键放入同一个哈希槽。

```js
const storage = createStorage({
  driver: redisDriver({
    base: "{unstorage}",
    cluster: [
      {
        port: 6380,
        host: "HOSTNAME",
      },
    ],
    clusterOptions: {
      redisOptions: {
        tls: { servername: "HOSTNAME" },
        password: "REDIS_PASSWORD",
      },
    },
  }),
});
```

**选项：**

- `base`: 用于所有键的可选前缀。可以用于命名空间。必须用作Redis集群模式的哈希前缀。
- `url`: 用于连接Redis的URL。优先于`host`选项。格式为`redis://<REDIS_USER>:<REDIS_PASSWORD>@<REDIS_HOST>:<REDIS_PORT>`
- `cluster`: 用于集群模式的Redis节点列表。优先于`url`和`host`选项。
- `clusterOptions`: 用于集群模式的选项。
- `ttl`: 所有项目的默认TTL（秒）。

请参见[ioredis](https://github.com/luin/ioredis/blob/master/API.md#new-redisport-host-options)以获取所有可用选项。

`lazyConnect`选项默认启用，以便在第一次Redis操作时建立连接。

**事务选项：**

- `ttl`: 在`setItem(key, value, { ttl: number /* seconds */ })`中支持。