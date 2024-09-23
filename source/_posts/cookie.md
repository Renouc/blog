---
title: cookie
date: 2024-09-21 14:27:48
tags:
  - cookie
---

Cookie 是直接存储在浏览器中的一小串数据。它们是 HTTP 协议的一部分，由 RFC 6265 规范定义。

Cookie 通常是由 Web 服务器使用响应 Set-Cookie HTTP-header 设置的。然后浏览器使用 Cookie HTTP-header 将它们自动添加到（几乎）每个对相同域的请求中。

document.cookie 属性用于读取和设置 Cookie。

### 读取 cookie

```js
consoloe.log(document.cookie); // "key1=value1; key2=value2; key3=value3"
```

### 设置 cookie

我们可以写入 document.cookie。但这不是一个数据属性，它是一个 [访问器（getter）](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/get)。对其的赋值操作会被特殊处理。

```js
document.cookie = "user=Tom;";
```

运行上述代码，会新添加一个 cookie。这是因为 document.cookie= 操作不是重写整所有 cookie。它只设置代码中提到的 cookie user。
