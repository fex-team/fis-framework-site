---
layout: post
title: page
category: fis-plus/user
---

## 页面模板

在模块中，用户可直接访问浏览的页面称为页面模板，文件在 **模块根目录/page/** 下。FIS提供提供了很多模板插件替换原生html标签，为页面模板开发提供使用。

* 通过[html插件](/userdoc/fis/%E6%8F%92%E4%BB%B6%E4%BD%BF%E7%94%A8#html)控制整体页面的输出，以及注册前端组件化框架。
* 通过[head插件](/userdoc/fis/%E6%8F%92%E4%BB%B6%E4%BD%BF%E7%94%A8#head)在模板解析运行时，控制加载同步静态资源使用。
* 通过[body插件](/userdoc/fis/%E6%8F%92%E4%BB%B6%E4%BD%BF%E7%94%A8#body)可在页面底部集中输出JS静态资源。

从demo中common模块的layout.tpl，可以了解到如何通过后端框架进行开发，组织整个页面:
{%raw%}

```smarty
<!DOCTYPE html>
{%* 使用html插件替换普通html标签，同时注册JS组件化库 *%}
{%html framework="common:static/mod.js" class="expanded"%}
    {%* 使用head插件替换head标签，主要为控制加载同步静态资源使用 *%}
    {%head%}
    <meta charset="utf-8"/>
        <meta content="{%$description%}" name="description">
        <title>{%$title%}</title>
        {%block name="block_head_static"%}{%/block%}
    {%/head%}
    {%* 使用body插件替换body标签，主要为可控制加载JS资源 *%}
    {%body%}
    {%block name="content"%}{%/block%}
    {%/body%}
{%/html%}
```

* 通过[require插件](/userdoc/fis/%E6%8F%92%E4%BB%B6%E4%BD%BF%E7%94%A8#require)加载静态资源，便于静态资源管理。
* 通过[script插件](/userdoc/fis/%E6%8F%92%E4%BB%B6%E4%BD%BF%E7%94%A8#script)管理JS片段，集中在页面底部加载。
* 通过[widget插件](/userdoc/fis/%E6%8F%92%E4%BB%B6%E4%BD%BF%E7%94%A8#widget)调用模板组件组织页面，处理对应的静态资源。

在demo-home模块中的index.tpl，加载页面模板对应的静态资源，通过模板组件组织页面:

```smarty
{%extends file="common/page/layout.tpl"%}
{%block name="block_head_static"%}
    <!--[if lt IE 9]>
        <script src="/lib/js/html5.js"></script>
    <![endif]-->
    {%* 模板中加载静态资源 *%}
    {%require name="home:static/lib/css/bootstrap.css"%}
    {%require name="home:static/lib/css/bootstrap-responsive.css"%}
    {%require name="home:static/lib/js/jquery-1.10.1.js"%}
{%/block%}
{%block name="content"%}
    <div id="wrapper">
        <div id="sidebar">
            {%* 通过widget插件加载模块化页面片段，name属性对应文件路径,模块名:文件目录路径 *%}
            {%widget
                name="common:widget/sidebar/sidebar.tpl"
                data=$docs
            %}
        </div>
        <div id="container">
            {%widget name="home:widget/slogan/slogan.tpl"%}
            {%foreach $docs as $index=>$doc%}
                {%widget
                    name="home:widget/section/section.tpl"
                    call="section"
                    data=$doc index=$index
                %}
            {%/foreach%}
        </div>
    </div>
    {%require name="home:static/index/index.css"%}
    {%* 通过script插件收集JS片段 *%}
    {%script%}var _bdhmProtocol = (("https:" == document.location.protocol) ? " https://" : " http://");
document.write(unescape("%3Cscript src='" + _bdhmProtocol + "hm.baidu.com/h.js%3F70b541fe48dd916f7163051b0ce5a0e3' type='text/javascript'%3E%3C/script%3E"));{%/script%}
{%/block%}
```

在模板框架中，对应的文件调用路径均为 ``modulename:相对模块根目录的相对路径`` ：

    //home为模块名，static/lib/css/bootstrap.css为绝对路径截取home模块根目录后的路径
    {%require name="home:static/lib/css/bootstrap.css"%}

### 前端模板

在FIS中，集成了[百度前端模板](http://baidufe.github.com/BaiduTemplate)。在编译过程中，会静态编译baidu template，不需要线上编译，提高页面运行效率。

在JS代码中，通过 **__inline** 方式进行编译处理前端模板。同时规定以 **tmpl** 为后缀的文件为前端模板，使用方式：

```javascript
//编译前
var demoTemplate = __inline('demo.tmpl');
//编译后
var demoTemplate = function (_template_object) {
       .......
};
```

在使用前端模板时需要注意的问题：

    //在__inline前端模板前加载template对象
    require("common:static/lib/template.js")
    //或者通过全局加载的方式
    {%require name="common:static/lib/template.js"%}

template的安装方式

    //在common的static/lib目录下执行下面命令
    $ lights install template

## 模板组件化

模板组件是能独立提供功能且能够复用的页面片段，所在规范目录 **模块根目录/widget/** ，模板组件以模板、JS组件、CSS组件组成（默认必须有模板）。

## widget定义

只要在widget目录下的Smarty模板即为模板组件，``目录下有与模板同名的JS、CSS文件FIS会自动添加依赖关系处理，在模板渲染时进行同步加载``。

### widget调用

调用一个模板组件需要通过widget语法，如我们想调用home下的section模板组件，则语法为

    //调用模板的路径为 modulename:模板在widget目录下路径
    {%widget name="home:widget/section/section.tpl" %}

## widget函数调用方式

widget插件可以直接调用某个smarty的function函数，使用方式为:

    //在模板中定义function函数
    {%function name="functionDemo"%}
           .............
    {%/function%}
    ------------------------------------
    //在模板中调用此函数
    {%widget call="functionDemo" %}
{%endraw%}