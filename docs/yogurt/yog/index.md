---
layout: post
title: Yog framework
category: yogurt/yog
---
{% raw %}

## 介绍

yog 是基于`kraken-js`的node后端框架，其集成了`bigpipe`，`fis静态资源管理`等功能，结合基于`fis`的前端解决方案`yogurt`提高前后端开发效率。

## 快速上手

_想直接看效果，可以下载demo查看[fex-team/yog-app](https://github.com/fex-team/yog-app)_

- [创建App目录](#创建App目录)
- [server.js](#serverjs)
- [创建一个controller](#创建一个controller)
- [创建一个页面模板](#创建一个页面模板)
- [运行](#运行)

### 创建App目录

一般yog的一个app需要`controllers`、`views`、`public`、`models`、`config`等几个目录。

下面创建它们;

```bash
# 创建配置文件，config.json可以配置中间件，views等信息
$ mkdir config
$ echo '{}' > config/config.json
# 创建controllers文件夹，此文件夹下的文件，yog启动的时候会自动load
$ mkdir controllers
# 创建模板目录views
$ mkdir views
# 创建静态资源目录
$ mkdir public
# App入口文件
$ touch server.js
$ tree
.
├── config
│   └── config.json
├── controller
├── public
├── server.js
└── views

3 directories, 1 file

```

### server.js

`server.js`是项目的入口文件，这个文件里面需要调用`yog`，创建一个App并初始化，监听一个端口。

把以下代码放到`server.js`里面

```javascript
var yog = require('yog'),
    app = require('express')(),
    port = process.env.PORT || 8000;


app.use(yog()); //启用yog

app.listen(port, function (err) {
    console.log('[%s] Listening on http://localhost:%d', app.settings.env, port);
});
```
这样整个入口文件已经写好，运行它就可以启动一个`yog`应用了。

可以从代码看出，使用`yog`非常简单，由于`keraken-js`基于express，`yog`也是，只需要需要使用中间件的方式启用`yog`即可。

```javascript
app.use(yog());
```

就这样你再也不用关心需要不需要`use`session中间件等问题，这些在`yog`中都准备好了，只需要使用即可。

对于`router`的一些改变，一般写`express`需要`app.get`或`app.post`来指定路由。如果不做拆解可能就写在`server.js`。
`keraken-js`比较好的解决了这个问题，它把`router`放到了`controller`里面。这样，多人开发页面的时候就不需要到同一个路由配置处进行更改，各个`controller`关心各自的`router`，`yog`也继承了这个特性。

以下说明如何写一个`controller`

### 创建一个controller

- 创建文件`controllers/index.js`
- 书写`controllers`逻辑

`controller`都需要`exports`出一个函数，参数是`router`实例。

```javascript
module.exports = function (router) {
    router.get('/', function (req, res) {
        res.end('hello world');
    });
};
```

当程序启动，访问`http://127.0.0.1/` 时就会在页面输出

```html
hello world
```

### 创建一个页面模板
每个app都需要页面模板，yog默认选用的模板引擎是`swig`，`swig`以高效、精悍、扩展方便著称。

- 创建文件`views/index.tpl`
    
    ```html
    <!doctype html>
    <html>
        <head>
            <meta charset="utf8" />
            <title> This is a test </title>
        </head>
        <body>
            Hello World!
        </body>
    </html>
    ```

- 使用`index.tpl`
    
    ```javascript
    //vi controllers/index.js
    module.exports = function (router) {
        router.get('/', function (req, res) {
            res.render('index');
        });
    };
    ```

### 运行
运行之前需要安装yog，为了管理方便，可以在app里面创建`package.json`，用`npm`进行管理项目中用到的组件。

package.json

```json
{
  "name": "myapp",
  "version": "0.0.1",
  "description": "myapp",
  "main": "server.js",
  "dependencies": {
    "yog": "~0.0.7"
  }
}
```

然后

```bash
$ npm install
```

安装app的依赖，这时候`yog`就会被安装到`app/node_modules`

最后，运行`server.js`

```bash
$ node server.js
```

## Configure

http://krakenjs.com/#configuration

## Router

http://krakenjs.com/#routes


## 模板 

yog 默认内置了[swig](http://paularmstrong.github.io/swig/)模板引擎

## 跟FIS结合使用

没错，上面的这些特性除了yog定制了目录结构，基本都是`kraken-js`的特性。有人就会问，那为啥还整个`yog`，就是为了跟FIS结合使用。

[FIS](http://fis.baidu.com)极大的提升了前端开发效率和运行效率。而且可以在其基础上比较简单的做一些高级特技，比如`bigpipe`。

好吧，其实 `yog`支持`bigpipe`、`FIS静态资源管理`这两个特性。更由于node可以并行输出性能一下就上去了（试试就知道）。

跟FIS结合使用的详细文档可以参见[fex-team/yogurt](https://github.com/fex-team/yogurt)

## bigpipe

姑且就不再解释这个名词了，网上解释的文章很多。

本节主要说明在yog中`bigpipe`如何使用。

在这之前先分享一下node的stream操作。node中不管是文件、网络链接都是一个stream。所以你完全可以一边从文件读取数据，一边返回给浏览器。做法很简单

```javascript

fs.readFile('./test.js', function (err, fstream) {
    fstreamt.pipe(responseWriter);
});

```

浏览器接收这种情况的输出时表现为，使用同一个链接一节一节的获取页面，这个就是`chunk`输出；

有了对chunk的原生支持，就可以把页面分为几个部分通过一个链接传输过来。而为了做到高效，后端对这几个部分的数据获取应该也是并行的。

在yog里面，可以很轻松的做到这一点，也就是所谓的bigpipe。


```javascript
res.bigpipe.bind('first', function (done) {
  fs.exists('./xxx', function (err) {
      done(err, {
      });
  });
}).bind('second', function (done) {
  db.find(function (err, res) {
      done(err, res);
  });
});

res.render('index.tpl', {});
```

上面这段代码就是一个使用bigpipe的后端代码；

再看看模板是如何写的；

```html
{%html%}
  ...

  {%widget "first.tpl" id="first"%}
  {%widget "second.tpl" id="second"%}
  ...
{%endhtml%}
```

- `first.tpl` 和 `second.tpl` 就是页面的两个片段

不难看出模板片段的数据是如何绑定的。

后端通过`bind`函数给某一个`id`的模板绑定数据，数据的获取可以是同步也可以是异步。

而输出`first`或`second`是用`chunk`方式输出。

## 示例

https://github.com/fex-team/yog-app

{% endraw %}