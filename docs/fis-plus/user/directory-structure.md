---
layout: post
title: fis-plus 目录规范
category: fis-plus/user
---

> 规范是辅助用户开发的利器，按照规范进行开发，可以极大的提升开发效率。
在FIS中，定义了一套默认的模块化开发规范。

## 站点目录结构

业务功能模块化，针对常见的业务模型，抽象出以下层级关系：

* ``站点(site)``：一般指能独立提供服务，具有单独二级域名的产品线。如旅游产品线或者特大站点的子站点（lv.baidu.com）。
* ``模块(module)``：具有较清晰业务逻辑关系的功能业务集合，一般也叫系统子模块，多个子系统构成一个站点。
* ``页面(page)``: 具有独立URL的输出内容，多个页面一般可组成子系统。
* ``组件(widget)``：能独立提供功能且能够复用的独立资源，它可以是独立的JS、CSS或者是由JS、CSS和页面组成的页面碎片。
* ``静态资源(static)``：非组件资源目录，包括模板页面引用的静态资源和其他静态资源（favicon,crossdomain.xml等）。
* ``插件(plugin)``: 模板插件目录(可选，对于特殊需要的产品线，可以独立维护)。
* ``测试数据(test)``: 页面对应的测试数据目录。

FIS规范定义了两类模块： **common模块**与 **业务模块**。

* ``common模块``: 为其他业务模块提供规范、资源复用的通用模块。
* ``业务模块``: 根据业务、URI等将站点进行划分的子系统站点模块。

站点整体目录结构示意：

```bash
|---site
|     |---common //通用子系统
|     |      |---config //smarty配置文件
|     |      |---page //模板页面文件目录，也包含用于继承的模板页面
|     |            └── layout.tpl
|     |      |---widget //组件的资源目录，包括模板组件,JS组件,CSS组件等
|     |      |     └── menu   //widget模板组件
|     |      |     |    └── menu.tpl
|     |      |     |    └── menu.js
|     |      |     |    └── menu.css
|     |      |     └── ui   //UI组件
|     |      |          └── dialog  //JS组件
|     |      |          |    └──dialog.js
|     |      |          |    └──dialog.css
|     |      |          └── reset //CSS组件
|     |      |               └── reset.css
|     |      |---static //非组件静态资源目录，包括模板页面引用的静态资源和其他静态资源
|     |      |---plugin //模板插件目录(可选，对于特殊需要的产品线，可以独立维护)
|     |      |---fis-conf.js //fis配置文件
|     |---module1 //module1子系统
|     |      |---test
|     |      |---config
|     |      |---page
|     |            └── index.tpl
|     |      |---widget
|     |      |---static
|     |      |     └── index //index.tpl模板对应的静态资源
|     |      |          └── index.js
|     |      |          └── index.css
|     |      |---fis-conf.js //fis配置文件

        ......
```

**为什么FIS要按照上述模块化方式定义规范呢？**

* 模块化是一种处理复杂系统分解成为更好的可管理模块的方式，它可以把系统代码划分为一系列职责单一，高度解耦且可替换的模块，系统中某一部分的变化将如何影响其它部分就会变得显而易见，系统的可维护性更加简单易得。

* 想了解的更多，可以查看[FIS模块化](http://fis.baidu.com/blog/%E5%89%8D%E7%AB%AF%E5%B7%A5%E7%A8%8B%E4%B9%8B%E6%A8%A1%E5%9D%97%E5%8C%96)

## 路径说明

1. ``页面(page)``：存放在 **模块根目录/page** 下，url访问路径为 **/模块名/page/页面名**，例如path_to_user_module/page/view.tpl，访问url为：/user/page/view。页面静态资源存储的位置为：

    ```bash
    tpl ：path_to_module/page/页面名.tpl
     js ：path_to_module/page/页面名.js
    css ：path_to_module/page/页面名.css
    ```

1. ``css组件``：一般来说，CSS组件是最简单的组件，它的存储方式为：

    ```bash
    #widget目录下的css文件皆为css组件，建议存放在widget/ui目录下
    css ：path_to_module/widget/ui/组件名/组件名.css
    ```

1. ``js组件``：支持AMD规范的js组件，js组件存储的方式为：

    ```bash
    #widget目录下的js文件皆为js组件，建议存放在widget/ui目录下
    js ：path_to_module/widget/ui/组件名/组件名.js
    ```

1. ``模板组件``：存放在 **模块根目录/widget** 下，每个widget包含至少一个与widget目录**同名**的tpl，同时可以有与widget **同名** 的js、css作为其静态资源。组件存储方式为：

    ```bash
    tpl ：path_to_module/widget/组件名/组件名.tpl
     js ：path_to_module/widget/组件名/组件名.js
    css ：path_to_module/widget/组件名/组件名.css
    ```

1. ``配置文件(fis-conf.js)``：fis配置文件存放在模块根目录下 **path_to_user_module/fis-conf.js** ，smarty配置文件存放在：

    ```bash
    conf：path_to_module/config/模块名/
    ```

1. ``smarty插件``：与smarty插件相关的都存放在plugin目录下，存储位置为：

    ```bash
    插件：path_to_module/plugin/
    ```

1. ``测试数据(test)``：fis开发环境允许在本地开发中设置测试数据进行调试，测试数据以页面模板为单位进行组织，其存储方式为：

    ```bash
    tpl：path_to_module/page/模块名/页面名.tpl
    data：path_to_module/test/page/页面名.json(或php)
    ```

## 组件化

模块的widget目录默认为组件目录，组件化按照代码的组织方式，分为以下三种：

* CSS组件：独立css代码片段。可以被其他css，js，模板引用。

* JS组件：独立js代码片段，JS组件可以封装CSS组件的代码。

* 模板组件：涉及代码最多，有模板代码，JS代码，css代码和HTML代码。 建议，模板组件中的js仅被这个widget使用，保持widget的独立。

下面的文档会详细介绍组件的使用方式。