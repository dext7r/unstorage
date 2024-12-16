----
icon: ph:browser-thin
---

# 浏览器

> 基于浏览器的存储。

## 本地存储

在 `localStorage` 中存储数据。

### 使用方法

::read-more{to="https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage"}
了解更多关于 `localStorage` 的信息。
::

```js
import { createStorage } from "unstorage";
import localStorageDriver from "unstorage/drivers/localstorage";

const storage = createStorage({
  driver: localStorageDriver({ base: "app:" }),
});
```

**选项：**

- `base`: 向所有键添加 `${base}:` 以避免冲突
- `localStorage`: 可选地提供 `localStorage` 对象
- `window`: 可选地提供 `window` 对象

## 会话存储

> 在 `sessionStorage` 中存储数据。

::read-more{to="https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage"}
了解更多关于 `sessionStorage` 的信息。
::

```js
import { createStorage } from "unstorage";
import sessionStorageDriver from "unstorage/drivers/session-storage";

const storage = createStorage({
  driver: sessionStorageDriver({ base: "app:" }),
});
```

**选项：**

- `base`: 向所有键添加 `${base}:` 以避免冲突
- `sessionStorage`: 可选地提供 `sessionStorage` 对象
- `window`: 可选地提供 `window` 对象

## IndexedDB

在 IndexedDB 中存储键值对。

### 使用方法

::read-more{to="https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API"}
了解更多关于 IndexedDB 的信息。
::

要使用它，您需要在项目中安装 [`idb-keyval`](https://github.com/jakearchibald/idb-keyval)：

:pm-install{name="idb-keyval"}

用法：

```js
import { createStorage } from "unstorage";
import indexedDbDriver from "unstorage/drivers/indexedb";

const storage = createStorage({
  driver: indexedDbDriver({ base: "app:" }),
});
```

**选项：**

- `base`: 向所有键添加 `${base}:` 以避免冲突
- `dbName`: 数据库的自定义名称。默认值为 `keyval-store`
- `storeName`: 存储的自定义名称。默认值为 `keyval`

::note
IndexedDB 是一个浏览器数据库。避免在服务器环境中使用这个预设。
::