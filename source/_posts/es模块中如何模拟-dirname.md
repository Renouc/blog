---
title: es模块中如何模拟__dirname
date: 2024-09-22 22:24:02
tags:
  - node
  - path
categories:
  - node
---

在 commonjs 中，全局变量中有 `__dirname` 可以获取当前的目录地址。

然而，如果你采用了 es module，那么 `__dirname` 就不存在了。

我们可以借助 `import.meta.url` 和 `path` 模块来模拟这个变量。

```js
import { fileURLToPath } from "url";
import { dirname } from "path";

const __filename = fileURLToPath(import.meta.url);
const __dirname = dirname(__filename);
```
