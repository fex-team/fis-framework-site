---
layout: post
title: css widget
category: fis-plus/user
---

> 在前端开发中，CSS资源占了很大一部分比例，在FIS中，我们将CSS资源分为组件和非组件类。CSS组件会绑定到同名JS组件、模板组件上，进行加载管理，用户不需要关心加载方式；非组件类CSS资源通过link标签、require插件方式进行加载。

## CSS组件化

在 **模块根目录/widget** 目录下的CSS资源，皆为组件。在模板组件中以及JS组件中对应同名的CSS组件会自动与模板组件、JS组件添加依赖关系，进行加载管理，用户不需要显示进行调用加载。

## 其他CSS

在非widget目录下的CSS资源，皆为非组件化资源。用户可以通过link标签、[require插件](smarty-plugin.html#require)等方式进行调用.

## 页面模板静态资源

``对应页面模板的同名静态资源，FIS会在页面自动进行加载``，用户不需要在页面中声明加载。

```bash
tpl ：模板根目录/page/页面名.tpl
js ：模板根目录/page/页面名.js
css ：模板根目录/page/页面名.css
```

## 模板组件静态资源

``与模板组件同名的静态资源，FIS会自动添加依赖关系，同时会对JS、CSS进行同步加载``。

```bash
tpl ：模板根目录/widget/widgetName/页面名widgetName.tpl
js ：模板根目录/widget/widgetName/widgetName.js
css ：模板根目录/widget/widgetName/widgetName.css
```

## Less资源

在FIS中默认配置了对less资源处理插件，less文件编译处理后变为css文件。
