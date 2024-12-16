---
icon: mdi:github
---

# GitHub

> 从远程 GitHub 仓库映射文件（只读）。

## 用法

此驱动程序一次获取所有可能的密钥，并将其缓存 10 分钟。由于 GitHub 的速率限制，强烈建议提供令牌。此设置仅适用于获取密钥。

```js
import { createStorage } from "unstorage";
import githubDriver from "unstorage/drivers/github";

const storage = createStorage({
  driver: githubDriver({
    repo: "nuxt/nuxt",
    branch: "main",
    dir: "/docs",
  }),
});
```

**选项:**

- `repo`: GitHub 仓库。格式为 `username/repo` 或 `org/repo` **（必填）**
- `token`: GitHub API 令牌。 **（推荐）**
- `branch`: 目标分支。默认是 `main`
- `dir`: 使用目录作为驱动程序根目录。
- `ttl`: 文件名缓存重新验证时间。默认是 `600` 秒（10 分钟）
- `apiURL`: GitHub API 域。默认是 `https://api.github.com`
- `cdnURL`: GitHub RAW CDN Url。默认是 `https://raw.githubusercontent.com`