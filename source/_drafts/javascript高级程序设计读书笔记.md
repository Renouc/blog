---
title: javascript高级程序设计读书笔记
tags: [前端]
---

《javascript 高级程序设计》是一本不错的书，戒躁戒躁，用心讲它读完。

<!-- more -->

# Javascript

## javascript 由三部分组成

- ECMAScript 核心语法 如：变量、函数、运算符、语句等

- DOM 提供了操作页面元素的接口，如：document、element、node 等

- BOM 操作浏览器原生行为，如：window、navigator、location、history 等

## 数据类型

- 基本类型：number、string、boolean、null、undefined、symbol、bigint

- 引用类型：object

## typeof

typeof 是操作符，不需要像函数那样带括号。

使用方法

```js
const name = "javascript";
console.log(typeof name); // string
```

typeof 返回的数据类型都是 string。

| 类型         | 值          |
| ------------ | ----------- |
| number       | "number"    |
| string       | "string"    |
| boolean      | "boolean"   |
| undefined    | "undefined" |
| function     | "function"  |
| object、null | "object"    |
| symbol       | "symbol"    |
| bigint       | "bigint"    |

## 声明关键字

var 关键字声明的变量是全局变量，会被挂载为 window 对象 的属性，只有函数作用域。并且有变量提升，在声明前可以使用，只是访问到的值是 undefine。

let 关键字声明的变量具有块级作用域。没有变量提升，在声明前使用会报错。

const 关键字声明的变量具有块级作用域，没有变量提升，在声明前使用会报错，并且不能再修改。

暂存性死区 使用 let 或 const 声明变量，在声明前使用会报错。这种现象叫做暂存性死区。
