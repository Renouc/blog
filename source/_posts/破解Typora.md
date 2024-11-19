---
title: 破解Typora
tags:
  - Typora
categories:
  - Typora
date: 2024-11-19 09:53:34
---

Typora 是一款 Markdown 编辑器，它支持 Windows、macOS 和 Linux。

<!-- more -->

### 下载

Typora 中文官网：https://typoraio.cn

### 破解

第一步找到 Typora 的安装目录，如 Mac 电脑可进行如下操作：
command+space 搜索 `/Applications/Typora.app/Contents/Resources/TypeMark/` 并打开文件夹

第二步继续进入 `page-dist` => `static` => `js` => `Licenselndex.180dd4c7.54395836.chunk.js`

第三步将 `e.hasActivated="true"==e.hasActivated` 更改为 `e.hasActivated="true"=="true"`
