---
layout: post
title: 配置
category: fis-plus/user
---

## 模块配置

每个模块都有对应的模块名，同时需要在配置文件中申明，配置如下。

```javascript
fis.config.set('namespace', 'common');
```


模块编译输出时默认都为utf8编码，如果需要做修改，可以进行如下配置：

```javascript
fis.config.set('project.charset', 'gbk');
```


对静态资源进行MD5戳编译时，默认长度都为7，当然你也可以进行修改：

```javascript
fis.config.set('project.md5Length', 8);

```

## Smarty配置

在fis-config.js配置文件中，可在setting节点下配置smarty节点，修改定界符。

```xml
fis.config.set('settings.smarty', {
    'left_delimiter'  : '<{',
    'right_delimiter' : '}>'
})
```

### 本地调试环境smarty定界符修改
在模块根目录下放置`smarty.conf`将其内容设置为：

```
left_delimiter='<{'
right_delimiter='}>'
```

_假设有多个模块，只需要在common模块根目录下放置`smarty.conf`_

## domain配置

设置静态资源域名，domain的值如果不是特殊需要，请不要以"/"结尾。

```xml
//fis-conf.js
//用法一
fis.config.set('roadmap.domain', 'http://s1.example.com, http://s2.example.com');

//用法二
fis.config.set('roadmap.domain', {
    //widget目录下的所有css文件使用 http://css1.example.com 作为域名
    'widget/**.css' : 'http://css1.example.com',
    //所有的js文件使用 http://js1.example.com 或者  http://js2.example.com 作为域名
    '**.js' : ['http://js1.example.com', 'http://js2.example.com']
});
```