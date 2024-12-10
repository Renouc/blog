---
title: yaml语法
tags:
  - null
categories:
  - null
date: 2024-12-10 16:58:06
---


yaml 是为配置文件设计的一种数据序列化格式。你可以使用 [yaml 在线转换器](https://nodeca.github.io/js-yaml/)，来验证你的 yaml 是否正确。

<!-- more -->

# 基本语法

- 大小写敏感
- 使用缩进表示层级关系
- 缩进不允许使用 tab，只允许空格
- 缩进的空格数不重要，只要相同层级的元素左对齐即可
- '#' 表示注释

# 数据类型

YAML 支持以下数据类型：

- 对象：键值对的集合，又称为映射（mapping）/ 哈希（hashes） / 字典（dictionary）
- 数组：一组按次序排列的值，又称为序列（sequence） / 列表（list）
- 纯量（scalars）：单个的、不可再分的值

## 对象

对象的一组键值对，使用冒号结构表示。

```yaml
key1: value1
key2: value2
```

转换成 json 为:

```json
{ "key1": "value1", "key2": "value2" }
```

还可以将所有键值对写成一个行内对象。

```yaml
user1: { name: Bob, age: 15 }

# 上面的写法等同于
user2:
  name: Jack
  age: 15
```

## 数组

以 `-` 开头的行表示构成一个数组：

```yaml
- item1
- item2
- item3
```

转换为 json 为：

```json
["item1", "item2", "item3"]
```

可以通过缩进来表示多维数组。

```yaml
- item1
- item2
- item3
-
 - item11
 - item22
 -
  - item111
```

转换为json为：
```json
["item1", "item2", "item3", ["item11", "item22", ["item111"]]]
```

数组内元素为对象
```yaml
- name: Jack
  age: 18
- {name: Ken, age: 20}
```

转换为json为：
```json
[
  {
    "name": "Jack",
    "age": 18
  },
  {
    "name": "Ken",
    "age": 20
  }
]
```

数组也可以写成行内的形式
```yaml
list1:
 - A
 - B
 - name: Jack
   age: 19
 # 等同于
list2: [A, B, {name: Jack, age: 19}]
```

## 纯量
纯量是最基本的、不可再分的值。
- 字符串
- 布尔值
- 整数
- 浮点数
- Null
- 时间
- 日期

```yaml
# 布尔值大小写不敏感
boolean:
 - true
 - false
 - TRUE
 - FALSE
 
# 浮点数 可以使用科学计数法
float:
 - 3.1415926
 - 1.2345e+4
 
int:
 - 123 # 十进制
 - 0b1 # 二进制
 - 0xff # 十六进制
 - 1_000_000_000
 
null:
 - null # 直接使用 null
 - ~ # 使用 ~ 表示 null
 
string:
 # 单引号会对特殊字符转义
 - 'hello\n world'
 
 # 双引号不会对特殊字符转义
 - "hello\n World" 
 
 # 字符串可以写成多行，从第二行开始，必须得缩进。换行符会被转为空格。
 - hello
   world
   
 # | 保留换行符，末尾只会保留一个
 - |
  hello
  world
  
 # |- 删除末尾的换行符
 - |-
  hello
  world
  
 # 保留末尾的换行符
 - |+
  hello
  world
  
 # 删除中间的换行
 - >
  hello
  world
  xixi

# 日期必须使用ISO 8601格式，即yyyy-MM-dd
date:
 - 2024-12-10
 
# 时间使用ISO 8601格式，时间和日期之间使用T连接，最后使用+代表时区
datetime:
 - 2024-12-10T15:02:31+08:00
```

转成json为：
```json
{
  "boolean": [true, false, true, false],
  "float": [3.1415926, 12345],
  "int": [123, 1, 255, 1000000000],
  "null": [null, null],
  "string": [
    "hello\\n world",
    "hello\n World",
    "hello world",
    "hello\nworld\n",
    "hello\nworld",
    "hello\nworld\n\n",
    "hello world xixi\n"
  ],
  "date": ["2024-12-10T00:00:00.000Z"],
  "datetime": ["2024-12-10T07:02:31.000Z"]
}
```
# 引用
& 锚点和 * 别名，可以用来引用:

```yaml
defaults: &defaults
  adapter:  postgres
  host:     localhost

development:
  database: myapp_development
  <<: *defaults

test:
  database: myapp_test
  <<: *defaults
```

相当于：

```yaml
defaults:
  adapter:  postgres
  host:     localhost

development:
  database: myapp_development
  adapter:  postgres
  host:     localhost

test:
  database: myapp_test
  adapter:  postgres
  host:     localhost
```

& 用来建立锚点（defaults），<< 表示合并到当前数据，* 用来引用锚点。

下面是另一个例子:

```yaml
- &showell Steve 
- Clark 
- Brian 
- Oren 
- *showell 
```
转换成json为：

```json
["Steve", "Clark", "Brian", "Oren", "Steve"]
```