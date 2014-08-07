---
layout: post
title: fis-plus 测试联调
category: fis-plus/user
---

## 调试环境安装

> 用户在无后端开发环境情况下，可依赖提供本地开发调试进行开发，包括本地调试环境、模拟转发模块、测试数据模块。

```bash
fisp server init //安装调试环境
fisp server start //开启调试服务器
```
安装FIS-PLUS本地运行环境后，执行 **fisp server open** 命令可打开本地运行环境目录

![本地运行环境目录]({{site.img}}/fis-plus/server.jpg)

## 本地url模拟

> FIS的rewrite模块。用于本地浏览时，url的转发，覆盖fis默认的url规则，模拟线上url规则，ajax请求，文件转发等

* [查看详细文档](./rewrite.html)

## 本地测试数据

> FIS-PLUS提供了本地调试数据功能，本地预览模板时会根据选择的测试数据进行展现，默认的smarty版本为3.1.13。

* [查看详细文档](./test-data.html)

## 线上调试
* [查看详细文档](./test-online.html)