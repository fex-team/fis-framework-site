---
layout: post
title: fis-plus smarty 插件
category: fis-plus/user
---

> FIS-PLUS提供一套基于smarty(version >= 3.1.13)插件的后台运行框架，针对静态资源管理、模板模块化、pagelet等功能。

### html

* 功能：代替\<html\>标签，设置页面运行的前端框架，以及控制整体页面输出。
* 属性值：framework及html标签原生属性值
* 是否必须：是
* 用法：在模板中替换普通\<html\>标签
{%raw%}

```smarty
{%html framework="home:static/lib/mod.js"%}
    ....
{%/html%}
```

![](/static/images/fis-plus/tpl1.jpg)

### head

* 功能：代替\<head\>标签，控制CSS资源加载输出。
* 属性值：head标签原生属性值
* 是否必须：是
* 用法：在模板中替换普通\<head\>标签

```smarty
{%html framework="home:static/lib/mod.js"%}
    {%head%}
        <meta charset="utf-8"/>
    {%/head%}
{%/html%}
```

![head](/static/images/fis-plus/tpl2.jpg)

### body

* 功能：代替\<body\>标签，控制JS资源加载输出。
* 属性值：body标签原生属性值
* 是否必须：是
* 用法：在模板中替换普通\<body\>标签


```smarty
{%html framework="home:static/lib/mod.js"%}
    {%head%}
        <meta charset="utf-8"/>
    {%/head%}
    {%body%}
        ....
    {%/body%}
{%/html%}
```

![body](/static/images/fis-plus/tpl3.jpg)

### script

* 功能：代替\<script\>标签，收集使用JS组件的代码块，控制输出至页面底部。
* 属性值：无
* 是否必须：在模板中使用异步JS组件的JS代码块，必须通过插件包裹
* 用法：在模板中替换普通\<script\>标签

```smarty
{%html framework="home:static/lib/mod.js"%}
    {%head%}
       <meta charset="utf-8"/>
       {*通过script插件收集加载组件化JS代码*}
       {%script%}
           require.async("home:static/ui/B/B.js");
       {%/script%}
    {%/head%}
    {%body%}
        ...
    {%/body%}
{%/html%}
```

![script](/static/images/fis-plus/tpl4.jpg)

### require

* 功能：通过静态资源管理框架加载静态资源。
* 插件类型：compiler
* 属性值：name(调用文件目录路径，与src属性二选一) | src(调用线上资源，与name属性二选一)
* 是否必须：是
* 用法：在模板中如果需要加载模块内某个静态资源，可以通过require插件加载，便于管理输出静态资源

```smarty
{%html framework="home:static/lib/mod.js"%}
    {%head%}
       <meta charset="utf-8"/>
       {*通过script插件收集加载组件化JS代码*}
       {%script%}
            require.async("home:static/ui/B/B.js");
       {%/script%}
    {%/head%}
    {%body%}
        {%require name="home:static/index/index.css"%}
        ...
    {%/body%}
{%/html%}
```

![require](/static/images/fis-plus/tpl5.jpg)

### widget

* 功能：调用模板组件，渲染输出模板片段。
* 插件类型：compiler
* 属性值：name(调用文件目录路径)
* 是否必须：是
* 用法：在模板中调用某个模板组件

```smarty
{%html framework="home:static/lib/mod.js"%}
    {%head%}
       <meta charset="utf-8"/>
       {*通过script插件收集加载组件化JS代码*}
       {%script%}
            require.async("home:static/ui/B/B.js");
       {%/script%}
    {%/head%}
    {%body%}
        {%require name="home:static/index/index.css"%}
        {%widget name="home:widget/A/A.tpl"%}
    {%/body%}
{%/html%}
```

![widget](/static/images/fis-plus/tpl6.jpg)

{%endraw%}