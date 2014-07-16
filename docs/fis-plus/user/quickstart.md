---
layout: post
title: fis-plus 快速入手
category: fis-plus/user
---

## 安装

fis-plus 的 自动化/辅助开发工具 被发布为一套 npm包，它对环境的要求是：

* 操作系统：任何能安装 nodejs 的操作系统
* node版本：>= v0.8.0
* jre版本：>= v1.5.0 【如果不需要本地调试服务器，可以忽略java环境要求】
* php-cgi版本：>= v5.0.0 【如果不需要本地调试服务器，可以忽略php-cgi环境要求】

### 安装nodejs

* [安装nodejs](http://nodejs.org/)

### npm

[npm](https://www.npmjs.org/)是nodejs的包管理工具。安装nodejs后，npm就自动一起安装了。

* 用nodejs写的模块都发布在npm上。[npm网站](https://www.npmjs.org/)
* 用户需要使用npm install命令来安装nodejs模块。更多npm使用，执行 npm -h 来查看
* ``由于npm经常被墙，安装fis的时候会出现速度过慢，或者安装不上的问题`` 。
* 可以通过 npm的 ``--registry`` 参数指定仓库。指定国内的npm镜像来解决npm被墙的问题。

例如： 

```bash
npm install some-npm-module -g --registry=国内镜像
```
* 下面提供一个国内镜像。其他镜像大家可以自己找找
* --registry=http://r.cnpmjs.org

### 安装fis

nodejs安装好后，命令行执行

```bash
npm install -g fis-plus
```

安装好fis之后，执行 fisp -v，如果看到下面信息，恭喜，你已拥有百度前端团队倾情打造的前端集成解决方案的开发调试环境啦！

```bash
➜  ~  fisp -v

  v0.6.8

 __/\\\\\\\\\\\\\\\__/\\\\\\\\\\\_____/\\\\\\\\\\\___
  _\/\\\///////////__\/////\\\///____/\\\/////////\\\_
   _\/\\\_________________\/\\\______\//\\\______\///__
    _\/\\\\\\\\\\\_________\/\\\_______\////\\\_________
     _\/\\\///////__________\/\\\__________\////\\\______
      _\/\\\_________________\/\\\_____________\////\\\___
       _\/\\\_________________\/\\\______/\\\______\//\\\__
        _\/\\\______________/\\\\\\\\\\\_\///\\\\\\\\\\\/___
         _\///______________\///////////____\///////////_____
```

### 安装lights

[lights](http://lights.baidu.com)是fis提供的包管理工具，托管了fis所有资源。是使用fis的时候，必不可少的利器。

```bash
npm install -g lights
```

### 安装Java

为了使用FIS本地调试服务器，需要安装java

* [安装java](http://java.com/)

### 安装php-cgi

为了使用FIS本地调试服务器，需要安装php-cgi

* [mac安装](https://gist.github.com/xiangshouding/9359739)
* [windows安装](https://gist.github.com/lily-zhangying/9295c5221fa29d429d52)
* [linux安装](https://github.com/fouber/install-php-cgi-1)

## 示例
_以下示例都是在命令行下操作的，如果你是windows用户，请打命令的时候忽略命令前的`$`，而且请打开`cmd`来执行这些操作_

已经准备好了一个fis-plus的前端项目，只需要经过以下四步，就可以完整运行这个项目，并看到结果。

- 初始化本地模拟环境
- 下载Demo
- 发布
- 预览

### 初始化本地模拟环境
为了前后端开发分离，来并行开发，fis-plus提供了一套本地环境模拟的工具，安装并初始化它后就能方便的模拟线上环境了。

```bash
$ fisp server init
```

###下载Demo
FIS的所有示例及其组件都用包管理工具`lights`进行管理，使用`lights`安装demo。

```bash
$ lights install pc-demo
```
其实fisp已经集成了`lights`的客户端。

和上面等值的用法；

```bash
$ fisp install pc-demo
```

### 发布

```bash
$ cd pc-demo
$ fisp release -r common
$ fisp release -r home
```

### 预览

```bash
$ fisp server start #启动服务器
```
启动服务的时候，启动成功后会自动打开浏览器，访问首页，这时候你应该打开demo首页，并和下图是一致的。

![]({{site.img}}/fis-plus/demo-index.png)


## 示例解说

自此，一个前端项目已经运行起来了。你可以看一下pc-demo的源码，其中包含两个模块

- common
- home

`模块`这个词会贯穿整个文档，以及整个`fis-plus`的使用。为什么会有模块这种东西？
当前端代码很多时，不得不面临分组件，分页面。为了发布迭代方便，不得不把它们分为不同的子系统。比如用户信息、首页、详情页等等。

`模块`就是一个子系统，而在fis项目中用`namespace`和`fis-conf.js`来区分。每一个模块会有一个配置文件`fis-conf.js`，还会取名不同的`namespace`。这主要是为了区分模块之前的静态资源。

继续进入home模块，可以看看有几个目录及其文件

```bash
.
├── fis-conf.js
├── page
├── server.conf
├── static
├── test
└── widget
```

无疑，这就是使用`fis-plus`需要遵循的目录规范，为什么要有目录规范，可能在网上可以找到很多答案，这里就不再赘述。

说一下这几个目录所代表的意思；

- page 页面模板
- widget 组件，模板组件，JS组件，CSS组件，会被组件化
- static 这个目录下放一些不需要组件化的公共库，比如`lazyload.js`
- test 防止一些测试数据，和`page`下的模板相对应，表明哪个模板用哪个数据文件进行渲染
- server.conf 这是一个很有用的文件，它里面可以配置url转发，可以方便在本地模拟`ajax`请求等。

_细心的你有可能发现了一个比较专业的词汇**组件化**，组件化的细节比较繁多，准备新开一节说明_

