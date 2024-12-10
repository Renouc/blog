---
title: docker快速入门
tags:
  - docker
categories:
  - docker
date: 2024-11-20 16:09:22
---

Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。

<!-- more -->

# image 文件

Docker 将程序代码及其依赖打包为 image 文件。通过 image 文件可以生成 Docker 容器。image 文件可以看作是容器的模板，Dokcer 通过 image 文件生成容器的实例。同一个 image 文件可以生成多个同时运行容器实例。

image 是二进制文件。实际开发中，一个 image 文件往往通过继承另一个 image 文件，加上一些个性化设置而生成。举例来说，你可以在 Ubuntu 的 image 基础上，往里面加入 Apache 服务器，形成你的 image。

```shell
# 列出本机的所有 image 文件。
docker image ls

# 删除 image 文件
docker image rm [imageName]
```

输出这段提示以后，hello world 就会停止运行，容器自动终止。

有些容器不会自动终止，因为提供的是服务。比如，安装运行 Ubuntu 的 image，就可以在命令行体验 Ubuntu 系统。

```shell
docker container run -it ubuntu bash
```

对于那些不会自动终止的容器，必须使用 docker container kill 命令手动终止。

```shell
docker container kill [containID]
```
