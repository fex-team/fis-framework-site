---
layout: post
title: Webapp
category: fis-plus/user
---

> 高性能Webapp解决方案，利用后端渲染方式，实现传统webapp前端局刷效果，实现一站式的体验效果，大幅提高性能和静态资源缓存命中率。

## 介绍
高性能Webapp解决方案，与传统webapp前端局刷（backbone等）相比，优势有：

* 采用后端渲染，不使用前端模板，大幅提高渲染性能
* 几乎不改变开发习惯，轻松上手和改造原有项目。

## 快速上手

第一步，需要安装[fis-plus][0]

> $ npm install -g fis-plus

第二步，下载示例代码

linux or Unix:

> $ wget https://github.com/xiangshouding/bigpipe.smarty/archive/master.zip

windows:

[下载](https://github.com/xiangshouding/bigpipe.smarty/archive/master.zip)

后解压，进入目录single

> $ cd bigpipe.smarty-master/single

第三步，使用安装的[fis-plus][0]编译发布项目

> $ fisp release -cmpr common

> $ fisp release -cmpr index

第四步，启动开发服务器

> $ fisp server start

第五步，安装本地测试框架

> $ fisp server install pc

第六步，打开浏览器访问

局部刷新: [http://127.0.0.1:8080/index/page/index]()


## 使用文档

### **引入插件**

Webapp插件主要包括：

* 后端解决方案插件 [Webapp解决方案的插件][1]。下载后放置到项目的plugin目录中。
* 前端加载器[loader][2]，[前端loader][2]依赖[lazyload.js][4]
* FIS组件化库[modjs][3]保持最新

{%raw%}
插件引入代码如下：

```smarty
{%html framework="common:static/lib/js/mod.js"%}
    {%head%}
        ...
        {%require name="common:static/lib/js/lazyload.js"%}
        {%require name="common:static/lib/js/BigPipe.js"%}
        ...
    {%/head%}
    {%body%}
        ...
    {%/body%}
{%/html%}
```

最后，发布这个项目；访问对应URL查看页面。


### **支持localstorage**

> 无线端使用localstorage本地缓存，可以减少请求数目，提高性能。高性能Webapp解决方案也内置了localstorage本地缓存方案。

只需要引入支持localstorage的FIS组件化库[mod-store.js][6]即可。

插件引入代码如下：

```smarty
{%html framework="common:static/lib/js/mod-store.js"%}
    {%head%}
        ...
        {%require name="common:static/lib/js/lazyload.js"%}
        {%require name="common:static/lib/js/BigPipe.js"%}
        ...
    {%/head%}
    {%body%}
        ...
    {%/body%}
{%/html%}
```


### **前端loader**

`局部刷新` 中FIS提供了一个可被异步请求的`后端框架`(以[smarty插件][0]的方式)；
[前端loader][5]。

前端loader提供接口`fetch`方法，来异步请求渲染一个widget。

```javascript
BigPipe.fetch(url, containerId);
```


例子

```javascript
BigPipe.fetch('/index/page/index?pagelets[]="pager"', 'pager');
```

表示请求`paglet_id="pager"`的widget，并把它渲染到页面的`<div id="pager"></div>`
内。

这个接口提供了异步请求渲染一个widget的能力。这样就可以实现局刷了。

### 整页切换
支持整个页面的切换，例如：A页面 -> B页面

    | A | B |
    | {%widget name="xxxx" pagelet_id="pager"%} | {%widget name="oooo" pagelet_id="pager"%} |

两个都有相同的pagelet_id的widget，整页切换。

提供`widget_block`来解决。只需要在layout里面使用widget_block
其他页面extends即可。

```smarty
{%widget_block pagelet_id="pager"%}
    {%block name="body"%}{%/block%}
{%/widget_block%}
```


整个页面就这样切换起来了。


### **页面管理库page.js**

以上接口在使用时需要考虑前端很多细节，为此提供了page.js这个页面管路口库。

page.js是跟[@donny](https://github.com/doith) 同学合作写的页面管理的前端库[page.js](https://github.com/xiangshouding/bigpipe.smarty/blob/master/single/common/static/lib/js/spljs/page.js)

+ 事件代理，代理需要局刷的URL, 绑定异步接口;
+ 前进后退控制， 使用pushState

提供接口

#### appPage.start()

```javascript
appPage.start(
    containerId: 'pager',   //pagelets渲染容器
    pagelets: 'pager',      //请求的pagelet
    validateUrl: /.*/i,     //符合这个规则的链接或者带data-href属性的元素进行事件代理
    cacheMaxTime: 1000      //每一个pagelet的缓存时间，视访问情况而定。
);
```


#### data-area

如果更新小范围的内容该如何办？

+ 只需在触发元素上添加`data-area`属性，


如;

```html
<a href="/xxxxx" data-area="left-bar">A</a>
```

当点击时回请求页面的`pagelet_id="left-bar"`的widget，并渲染到当前页面的`<div id="left-bar"></div>`内。

#### appPage.redirect()

```javascript
appPage.redirect(
    "/index/page/index",
    {
        "pagelets": "test"     // 需要请求的pagelet
        "containerId": "xxx"   // pagelet渲染的容器
    }
);
```

{%endraw%}
如果还有一些小的pagelet（widget）没有考虑到，可以用这个接口做加载。


[0]: https://github.com/xiangshouding/bigpipe.smarty "BigPipe.smarty"
[1]: https://github.com/xiangshouding/fis-smarty-bigpipe-plugin "quickling plugin"
[2]: https://github.com/xiangshouding/bigpipe.smarty/blob/master/lazyrender/static/BigPipe.js "loader"
[3]: https://github.com/zjcqoo/mod "modjs"
[4]: https://github.com/xiangshouding/bigpipe.smarty/blob/master/lazyrender/static/lazyload.js "lazyload.js"
[5]: https://github.com/xiangshouding/bigpipe.smarty/blob/master/single/common/static/lib/js/BigPipe.js "loader"
[6]: https://github.com/xiangshouding/mod-store.js "mod-store.js"
