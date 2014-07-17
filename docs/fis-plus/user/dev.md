---
layout: post
title: fis-plus 本地开发
category: fis-plus/user
---

## 三条开发命令

> fis-plus扩展fis三条开发命令，执行fisp -h查看命令行帮助.

```bash
Usage: fis-plus <command>

Commands:

  release     build and deploy your project
  install     install components and demos, create `widget` and `module`
  server      launch a php-cgi server

Options:

  -h, --help     output usage information
  -v, --version  output the version number
  --no-color     disable colored output
```
* [查看详细文档](./command.html)

## 目录规范

> 规范是辅助开发的利器，按照规范进行开发，可以极大的提升开发效率。在FIS-PLUS中，定义了一套默认的模块化开发规范。

* [查看详细文档](./directory-structure.html)

## 三种语言能力

> FIS扩展前端开发中三种语言能力，提供前端开发最精简完整的编译工具集合。

* [查看详细文档](http://fis.baidu.com/docs/more/fis-standard.html)

## 模板开发

> 扩展多个Smarty插件，提供组件化开发机制，并切实现静态资源管理，极大提升页面渲染性能，简化开发流程。

* [Page 开发](./page.html)
* [模板组件化开发](./page.html)

### Smarty插件列表

* [html](./smarty-plugin.html#html)
* [head](./smarty-plugin.html#head)
* [body](./smarty-plugin.html#body)
* [script](./smarty-plugin.html#script)
* [require](./smarty-plugin.html#require)
* [widget](./smarty-plugin.html#widget)

## Js&Css

> 提供JS & CSS组件化开发方式，极大提高代码可维护性和并行开发能力。

* [Js 开发](./js-widget.html)
* [Css 开发](./css-widget.html)

## 自动化工具

> 拥有所有FIS自动化能力，同时针对Smarty+php，集成多个插件，实现：模板xss修复，模板压缩,css sprite图片合并等功能。

* [模板xss修复](./xss.html)
* [模板压缩](./xss.html)
* [css sprites 图片合并](./xss.html)
* [查看更多插件](http://fis.baidu.com/docs/advance/plugin-list.html)

## 配置

> 修改fis-conf.js文件，即可更新项目配置。FIS-PLUS默认配置简单完整，以下列出用户需要关注的配置。更多配置信息请访问FIS网站。

* [Smarty 配置](./config.html#smarty)
* [Doamin 配置](./config.html#smarty)
* [插件 配置](./plugin.html)
* [打包 配置](./pack.html)
* [autopack 自动打包服务](http://solar.baidu.com/autopack)
* [查看更多FIS服务](http://solar.baidu.com/)
