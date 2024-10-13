---
title: php快速入门
tags:
  - php基础
categories:
  - php
date: 2024-10-13 11:52:28
---

PHP 是一个通用的开源脚本语言，用于在服务器端创建动态网页。可以实现验证、数据库、文件处理、动态网页、动态脚本等功能。在 PHP 文件中，可以书写 html、css、js、php 等语法。

### 启动服务

```sh
php -S localhost:8080
```

执行上述命令会在当前目录下启动一个服务，使用浏览器访问相应的地址即可访问到对应的资源

```sh
php --help
```

可以通过上述命令查看更多的用法

### 输出语句

echo 可以输出一个或多个字符串，字符串中可以包含 HTML 标签，可以加括号

```php
<?php
echo "<p>hello world</p>";
echo "<p>hello</p>", "<p>world</p>";
echo ("hello world" . "你好世界");
?>
```

print 与 echo 类似，但只接受一个字符串，print 有返回值并且始终为 1。

```php
print "<p>嘻嘻嘻嘻</p>";
```

### 响应资源

#### 页面

在项目目录中创建一个 test.php 文件

```sh
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <h1>Hello PHP</h1>
    <?php
    echo "<p>Hello PHP</p>"
    ?>
</body>·

</html>
```

启动服务

```sh
php -S localhost:8080
```

启动服务之后，访问`http://localhost:8080/test.php`，你会发现输出语句中的内容被渲染成了 HTML 标签。你完全可以当成在写 html，只不过增添了 php 的扩展使得功能更加强大。

> 注意：php 文件是在服务器处理好了之后才返回的，返回的结果中已经不存在 php 相关的代码，全部已经转换为前端可以识别的 html、css、js。

#### 接口

在项目中创建一个 api.php 文件

```php
<?php
header("Content-Type: application/json;");

// 获取请求方法
$method = $_SERVER['REQUEST_METHOD'];

switch ($method) {
    case "GET":
        $data = ["id" => 1, "name" => "张三", "age" => 18];
        http_response_code(200);  // 设置响应状态码为 200 OK
        $response = $data;
        break;

    default:
        http_response_code(405);  // 设置响应状态码为 405 Method Not Allowed
        $response = ["error" => "Method Not Allowed"];
        break;
}

echo json_encode($response);
```

此时，访问`http://localhost:8080/api.php`，返回的结果为 json 字符串，内容如下：

```json
{
  "id": 1,
  "name": "张三",
  "age": 18
}
```

#### index.php

当你的请求路径只写到目录时，会自动访问该目录下的 index.php 文件

- 访问 http://localhost:8080，相当于 http://localhost:8080/index.php
- 访问 http://localhost:8080/api，相当于 http://localhost:8080/api/index.php
