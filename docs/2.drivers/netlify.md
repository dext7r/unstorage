---
icon: teenyicons:netlify-solid
---

# Netlify Blobs

> 在 Netlify Blobs 中存储数据。

在 [Netlify Blobs](https://docs.netlify.com/blobs/overview/) 存储中存储数据。此功能支持 [edge](#using-in-netlify-edge) 和 Node.js 函数运行时，以及在构建过程中使用。

::read-more{title="Netlify Blobs" to="https://docs.netlify.com/blobs/overview/"}
::

## 用法

```js
import { createStorage } from "unstorage";
import netlifyBlobsDriver from "unstorage/drivers/netlify-blobs";

const storage = createStorage({
  driver: netlifyBlobsDriver({
    name: "blob-store-name",
  }),
});
```

您可以通过将 `deployScoped` 选项设置为 `true` 来创建一个部署范围的存储。这意味着该部署只能访问其自己的存储。该存储与部署一起管理，具有相同的部署预览、删除和回滚功能。在构建过程中，这是必需的，因为构建只能访问部署范围的存储。

```js
import { createStorage } from "unstorage";
import netlifyBlobsDriver from "unstorage/drivers/netlify-blobs";

const storage = createStorage({
  driver: netlifyBlobsDriver({
    deployScoped: true,
  }),
});
```

要使用，您需要在项目中将 `@netlify/blobs` 安装为依赖项或开发依赖项：

```json
{
  "devDependencies": {
    "@netlify/blobs": "latest"
  }
}
```

**选项：**

- `name` - 要使用的存储名称。必要时会创建。除部署范围的存储外，这是必需的。
- `deployScoped` - 如果设置为 `true`，则存储的范围仅限于该部署。这意味着它仅在该部署可用，并将随之删除或回滚。
- `siteID` - 在构建过程中是必需的，可以通过 `constants.SITE_ID` 访问。在运行时会自动设置。
- `token` - 在构建过程中是必需的，可以通过 `constants.NETLIFY_API_TOKEN` 访问。在运行时会自动设置。

**高级选项：**

这些通常不需要，但可以用于高级用例或单元测试中。

- `apiURL`
- `edgeURL`

## 在 Netlify edge 中使用

在 Netlify edge 函数中使用 Unstorage 时，您应该使用 URL 导入。如果您在框架中编译代码，则不适用 - 只适用于创建自己的 edge 函数。

```js
import { createStorage } from "https://esm.sh/unstorage";
import netlifyBlobsDriver from "https://esm.sh/unstorage/drivers/netlify-blobs";

export default async function handler(request: Request) {

  const storage = createStorage({
    driver: netlifyBlobsDriver({
      name: "blob-store-name",
    }),
  });

  // ...
}
```

## 从 Netlify Blobs beta 更新存储

`@netlify/blobs` 版本 `7.0.0` 中对全局 blob 存储的存储方式进行了更改，这意味着在您迁移之前，您将无法访问由旧版本创建的全局存储中的对象。这不会影响部署范围的存储，也不会影响新版本创建的对象。您可以通过在项目目录中使用最新版本的 Netlify CLI 运行以下命令来迁移旧存储中的对象：

```sh
netlify recipes blobs-migrate <name of store>