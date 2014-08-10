---
layout: post
title: js widget
category: fis-plus/user
---

> 在前端开发中，JS资源占了很大一部分比例，在FIS中，我们将JS资源分为组件和非组件类。组件类JS资源可以通过前端组件化框架进行资源加载，同时会进行组件化包装。非组件类JS资源，用户可以通过同步script标签加载方式或通过require插件方式加载。

## JS组件化

在 **模块根目录/widget/** 下的JS资源皆为组件化资源，可以通过require和require.async进行调用，则在编译处理过程中会进行组件化封装。

## modjs前端组件化框架

modJS是一套的前端模块加载解决方案。与传统的模块加载相比，modJS会根据产品实际使用场景，自动选择一种相应的方案，使最终的实现非常轻量简洁。
作为FIS前端组件化框架，完全遵循AMD规范，用户可以通过lights进行安装

    lights install modjs

同时在开发中需要使用modjs,则需要通过[模板插件语法html](/userdoc/fis/%E6%8F%92%E4%BB%B6%E4%BD%BF%E7%94%A8#html)进行注册。

## 组件化封装

modjs使用define来定义一个模块：

    define (id, factory)

``在平常开发中，我们只需写factory中的代码即可，无需手动定义模块``。发布工具会自动将模块代码嵌入factory的闭包里。

factory提供了3个参数：require, exports, module ，用于模块的引用和导出。

在编译处理过程中会对JS文件进行组件化define包装处理:

* JS源码：

```javascript
//common/widget/menu/menu.js
var $ = require('common:widget/jquery/jquery.js');

exports.init = function() {
    $('.menu-ui ul li a').click(function(event) {
        var self = this;
        $('.menu-ui ul li a.active').removeClass('active');
        $(self).addClass('active');
        event.preventDefault();
    });
};
```
* 编译后代码：

```javascript
define('common:widget/menu/menu.js', function(require, exports, module){
    var $ = require('common:widget/jquery/jquery.js');
    exports.init = function() {
        $('.menu-ui ul li a').click(function(event) {
            var self = this;
            $('.menu-ui ul li a.active').removeClass('active');
            $(self).addClass('active');
            event.preventDefault();
        });
    };
});
```

## 组件化调用

* modJS的发布工具会保证你的程序在使用之前，所有依赖的模块都已加载。因此当我们需要一个模块时，只需提供一个模块名即可获取：

 **require** (id)

    ```javascript
    //id可为相对路径，或FIS中组件调用路径 模块名:文件所在widget中路径
    require("common:widget/ui/a/a.js")
    ```


 因为所需的模块都已预先加载，因此require可以立即返回该模块。

* 考虑到有些模块无需在启动时载入，因此modJS提供了可以在运行时``异步加载模块的接口``：

 **require.async** (names, callback)

 names可以是一个id，或者是数组形式的id列表。

 当所有都加载都完成时，callback被调用，names对应的模块实例将依次传入。

 使用require.async获取的模块不会被发布工具安排在预加载中，因此在完成回调之前require将会抛出模块未定义错误。

    ```javascript
    //id可为相对路径，或FIS-Plus中组件调用路径 模块名:文件所在widget中路径
    require.async(["common:widget/menu/menu.js"],function(menu){
          menu.init()
    })
    ```

* [了解更多modjs](/userdoc/fis/modjs)

## 其他JS

在非widget目录下的JS资源，皆为非组件化资源。用户可以通过script标签、[require插件](/userdoc/fis/%E6%8F%92%E4%BB%B6%E4%BD%BF%E7%94%A8#require)等方式进行调用.

### 页面模板静态资源

对应页面模板的同名静态资源，FIS-Plus会在页面自动进行加载，用户不需要在页面中声明加载。

```bash
tpl ：模板根目录/page/页面名.tpl
js ：模板根目录/page/页面名.js
css ：模板根目录/page/页面名.css
```

## 模板组件静态资源

与模板组件同名的静态资源，FIS会自动添加依赖关系，同时会对JS、CSS进行同步加载。

```bash
tpl ：模板根目录/widget/widgetName/widgetName.tpl
js ：模板根目录/widget/widgetName/widgetName.js
css ：模板根目录/widget/widgetName/widgetName.css
```
