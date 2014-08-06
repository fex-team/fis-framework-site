---
layout: post
title: quickling
category: fis-plus/user
---

> Quickling适用于网络高延迟、低带宽场景的解决方案。

* 保证首屏及核心功能最快展现，使得展示核心功能所需要获取的数据、生成的html文档大小、资源加载量、渲染工作量最小化；
* 提高服务端的渲染效率和并行度，保证功能不会受到慢数据模块的影响；
* 支持page cache和用户缓存控制，可以避免大量的服务器端重复计算和客户端重复渲染

## 背景

> **Quickling**这个词诞生自[facebook Web优化方案](http://www.slideshare.net/ajaxexperience2009/chanhao-jiang-and-david-wei-presentation-quickling-pagecache)，它指的是页面的某一个块可以通过Ajax请求，包括这块使用到的静态资源，然后通过JSON方式返回给前端加载器，前端加载器先加载静态资源然后渲染块，这样得到一个可展示的页面局部，可以把它放到当前页的任何地方。

**Quickling解决方案**也使用相同的原理。得益于FIS 2.0，我们很轻松就可以搞定整个逻辑的实现。

## 解决方案特性

>介绍Quickling是如何工作。解决方案特性：

+ **A** 支持任意一个widget被异步请求，请求内容包括渲染好的HTML及静态资源
+ **B** 当widget指定为异步请求时，渲染引用此widget模版时不会渲染此widget，降低后端渲染模版压力。

## 使用场景
使用场景一栏，主要给大家展示一些案例，来引导理解整个解决方案。

### 案例一

项目A中使用方案提供的最初使用前端模板实现webapp一站式效果，但是前端渲染的形式，在展示的时候后端获取数据分为两步，展现页面时只能等数据拿到以后才能进行展现，而恰巧获取数据时比较慢，导致页面出现卡顿。那么我们用**Quickling解决方案**如何解决这个问题呢？

答案很简单，展现页面的时候也分为两步走，第一次渲染的时候拿到比较重要那块的数据，并渲染对应的部分页面。再发起一次异步请求，请求剩下的部分页面。这样至少用户不会感觉到卡顿。是不是看着似曾相识，这个就好比纯的WebApp在渲染一个页面时，请求两次数据并渲染页面一样。但这个是后端模板层面支持的。

### 案例二

项目B主要服务于东南亚地区，这些国家的网络有个特点，那就是**慢**，有IPHONE并使用移动号的同学拿出手机访问一下某网站试试，就那种感觉。通过项目B同学的反馈以及统计数据显示，下载HTML的速度都慢的可怜。还有一个问题并发时下载资源之间抢带宽，阻塞页面的渲染。

### 问题
> 总结一下俩问题上面两个案例的问题

+ html太大，导致下载太慢
+ 资源抢带宽，阻塞页面渲染

那通过 **Quickling解决方案** 如何解决问题呢。可以通过，

+ 整个页面多次渲染，第一次访问或者刷新时只渲染首屏，这样展示首屏的时候就减少了很多html。下载变快了
+ 拆分逻辑，把基础功能的css内联，增强功能的css在一定条件下触发请求，js进行异步加载。这样控制后页面渲染就变快了。

### 总结

**案例一** 和 **案例二** 中可以看到，Quickling解决方案很好的解决了这些遇到的问题，而案例中说到的情况就是方案已知的适用场景，其他场景还有待发现。

## 使用方法
> 很多同学到这里就有疑问了，如此复杂的请求方式，一个页面可以分块请求，是不是需要在开发的时候实现很多东西，维护起来很麻烦。答案是 **否定** 的。整个方案依托于FIS 2.0的前端架构思想，从目录结构到静态资源管理。只需要做很小的工作就瞬间享受到 **Quickling解决方案** 带来的新特性。

首先得有一个后端模板是Smarty的项目，并且是使用FIS制定的目录规范以及用FIS编译。目录结构是这样的；

```bash
├── build.sh
├── config
├── fis-conf.js
├── page
├── static
├── test
└── widget
```

每个目录放些什么，就不一一说明了，见[FIS2.0文档](http://fe.baidu.com/doc/fis/2.0/user/)。我们只关注 **widget** 和 **page** 。

假设有一个widget **widget_A** ，包括一个模板文件widget_A.tpl和一个js文件widget_A.js还有一个css文件widget_A.css。有个页面 **index.tpl** 要使用这个widget。

```bash
├── page
│   └── index.tpl
└── widget
    └── widget_A
        ├── widget_A.js
        ├── widget_A.css
        └── widget_A.tpl
```

网站展示时渲染 **index.tpl** ，widget_A是页面中的一部分。

{%raw%}

```html
//index.tpl
{%widget name="demo:widget/widget_A/widget_A.tpl"%}
```
当页面被渲染时，widget_A就展现在页面上了。

```html
<html>
...
    <link href="widget_A.css" rel="stylesheet" type="text/css" />
....
    <div> 我是widget_A </div>
....
    <script src="widget_A.js" type="text/javascript"></script>
</html>
```

上面是正常的使用方式，就像**方案二**中说到的，如何让渲染index.tpl时不展示**widget_A**呢。

```html
{%widget name="demo:widget/widget_A/widget_A.tpl" mode="quickling" pagelet_id="widget_A"%}
```
OK，改造完成。
加了`mode="quickling"`和`pagelet_id="widget_A"`这俩参数。
这时候渲染页面的结果是什么呢？

```html
<html>
.....
<textarea class="fis_g_bigrender" style=“display:none”>BigPipe.asyncLoad({id: "widget_A"})</textarea>
<div id="widget_A"></div>
.....
</html>
```

{%endraw%}
如上代码，做了俩事情。

1. 挖了个坑`<div id="widget_A"></div>`，异步请求回来的widget_A的html就放在这个坑了。
2. 在textarea里面打了一个JS函数，这个思路来自bigrender，可以在页面滚动到那个部位才去拉取数据。

等页面渲染完后，开发的同学需要做什么，他只需要把textarea里面的代码根据自己的需求执行就成，比如滚轮滚那个地方，domready后。。。这个自己决定。

说到这里我想你也知道如何使用了。

**使用步骤** ：

+ widget调用的时候设定这个widget的 **渲染模式** 为`quickling`，`mode="quickling"`
+ widget调用的时候设定pagelet_id, `pagelet_id="widget_A"`
+ 运行时，取出class="fis_g_bigrender"中包含的代码，运行它
+ 页面引入前端加载器[BigPipe.js](https://github.com/xiangshouding/bigpipe.smarty)
+ 项目中使用方案提供的[smarty插件](https://github.com/xiangshouding/fis-smarty-bigpipe-plugin)

**相关资源** ：

+ [demo](https://github.com/xiangshouding/bigpipe.smarty)
+ [Smarty插件](https://github.com/xiangshouding/fis-smarty-bigpipe-plugin)


## 优点和缺点
有了使用场景而且也知道如何使用，那现在开始总结一下它到底有哪些优点，事物都是双面的当然也有缺点。这栏总结一下整个方案的优缺点。按照一贯的做法，先说优点。

### 优点
+ **灵活** 页面widget可以灵活请求
+ **可维护性高** FIS用户项目都是组件化的，维护肯定是最好的
+ **使用简单** 只需要关注那些页面部分想后展示、具体展示的时机
+ **能解决特定问题**  案例一和案例二已经说明了这一点。

### 缺点
+ 增加了请求  一个页面渲染，如果某一个widget显然模式是“quickling”，那么渲染页面就会多一次请求
+ 增加了服务器负担

**性能本来就是一个折中，方案有优缺点，就看具体场景需要了。**
