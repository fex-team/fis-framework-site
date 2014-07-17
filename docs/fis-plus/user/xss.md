---
layout: post
title: 自动化工具
category: fis-plus/user
---

## 模板XSS修复工具

* 功能：对Smarty模板进行XSS校验修复。
* 默认开启：true(已默认配置)
* 配置:
{%raw%}

```javascript
fis.config.set('modules.optimizer.tpl', 'smarty-xss');

fis.config.set('settings.optimizer.smarty-xss', {
    'escapeMap' : {
        'js' : 'f_escape_js',  //配置js_escape指定js转义插件的名称
        'html' : 'f_escape_xml', //配置html_escape指定对于xml转义插件的名称
        'data' : 'f_escape_data', //配置data_escape指定data转义插件的名称
        'path' : 'f_escape_path', //配置path_escape指定url、path转义插件的名称
        'event' : 'f_escape_event', //配置event_escape指定event转义插件的名称
        'no_escape' : 'escape:none' //不需要添加转义的变量标志
    },
    leftDelimiter' : '{%', //smarty左定界符
    'rightDelimiter' : '%}', //smarty右定界符
    'xssSafeVars' :[          //配置白名单
        'fis_safe'      //支持正则
    ]
});
```

## 图片合并工具

* 功能：对
* 环境要求：依赖native插件，[node-images](https://github.com/xiangshouding/node-images) 环境需要符合个插件的要求。(OS X、Windows提供了二进制包)
* 默认开启：true
* 使用文档：[用户文档](https://github.com/fex-team/fis-spriter-csssprites)
* 配置：

```javascript
fis.config.set('settings.spriter.csssprites', {
    //图之间的边距
    margin: 10
});
```

## 模板压缩工具

* 功能：对Smarty模板进行压缩。
* 默认开启：true(已默认配置)
* 配置:

```javascript
fis.config.set('modules.optimizer.tpl', 'html-compress');

```

* 详细使用文档：[用户文档](https://github.com/wangcheng714/fis-optimizer-html-compress)

## JS压缩工具

* 功能：通过uglify-js对JS文件进行压缩。
* 默认开启：true(已默认配置)
* 配置:

```javascript
fis.config.set('modules.optimizer.js', 'uglify-js');
```


## CSS压缩工具

* 功能：通过clean-css对CSS文件进行压缩。
* 默认开启：true(已默认配置)
* 配置:

```javascript
fis.config.set('modules.optimizer.css', 'clean-css');
```
{%endraw%}