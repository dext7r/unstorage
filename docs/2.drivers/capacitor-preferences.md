---
icon: nonicons:capacitor-16
---

# 电容器首选项

> 通过电容器首选项 API 在移动设备上存储数据或在网络上使用本地存储。

::read-more{to="https://capacitorjs.com/docs/apis/preferences"}
了解更多关于电容器首选项 API 的信息。
::

## 使用方法

要使用此驱动程序，您需要在您的电容器项目中安装并同步 `@capacitor/preferences`：

:pm-install{name="@capacitor/preferences"}
:pm-x{command="cap sync"}

用法：

```js
import { createStorage } from "unstorage";
import capacitorPreferences from "unstorage/drivers/capacitor-preferences";

const storage = createStorage({
  driver: capacitorPreferences({
    base: "test",
  }),
});
```

**选项：**

- `base`: 将 `${base}:` 添加到所有键以避免冲突