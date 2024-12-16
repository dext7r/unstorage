---
icon: simple-icons:upstash
---

# Upstash

> 在 Upstash Redis 数据库中存储数据。

## 用法

::read-more{to="https://upstash.com/"}
了解更多关于 Upstash 的信息。
::

::note
Unstorage 内部使用 [`@upstash/redis`](https://github.com/upstash/upstash-redis) 连接到 Upstash Redis。
::

要使用它，您需要在项目中安装 `@upstash/redis`：

:pm-install{name="@upstash/redis"}

与 Upstash Redis 的用法：

```js
import { createStorage } from "unstorage";
import upstashDriver from "unstorage/drivers/upstash";

const storage = createStorage({
  driver: upstashDriver({
    base: "unstorage",
    // url: "", // 或设置 UPSTASH_REDIS_REST_URL 环境变量
    // token: "", // 或设置 UPSTASH_REDIS_REST_TOKEN 环境变量
  }),
});
```

**选项：**

- `base`: 可选前缀，用于所有键。可用于命名空间。
- `url`: 您的 Upstash Redis 数据库的 REST URL。请在 [Upstash Redis 控制台](https://console.upstash.com/redis/) 中查找。驱动程序默认使用 `UPSTASH_REDIS_REST_URL` 环境。
- `token`: 用于与您的 Upstash Redis 数据库进行身份验证的 REST 令牌。请在 [Upstash Redis 控制台](https://console.upstash.com/redis/) 中查找。驱动程序默认使用 `UPSTASH_REDIS_REST_TOKEN` 环境。
- `ttl`: 默认的 TTL，单位为 **秒**，适用于所有项目。

有关所有可用选项，请参见 [@upstash/redis 文档](https://upstash.com/docs/redis/sdks/ts/overview)。

**事务选项：**

- `ttl`: 支持 `setItem(key, value, { ttl: number /* seconds */ })`。