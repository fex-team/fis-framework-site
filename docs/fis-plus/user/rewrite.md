---
layout: post
title: url 转发模块
category: fis-plus/user
---

## 简介
FIS的rewrite模块。用于本地浏览时，url的转发，覆盖fis默认的url规则，模拟线上url规则，ajax请求，文件转发等。

## 基础配置
在模块目录下通过配置文件server.conf文件进行配置，配置方式是：

rewrite和redirect开头的会被翻译成一条匹配规则，自上而下的匹配。所有非rewrite和redirect开头的会被当做注释处理。

```php
rewrite ： 匹配规则后转发到一个文件
template ： 匹配规则后转发到一个模板文件，但url不改变，只针对模板
redirect ： 匹配规则后重定向到另一个url

rewrite ^\/news\?.*tn\=[a-zA-Z0-9]+.* app/data/news.php
template ^\/(.*)\?.*  /home/page/index.tpl
redirect ^\/index\?.* /home/page/index
```

## 配置文件说明

1. 配置文件每一行为一条规则。
2. 格式为：匹配类型 (空格) 匹配url正则 (空格) 命中正则后的目的文件路径或者url。
   rewrite、redirect和template是fis默认的三种匹配类型。
3. **rewrite** ： 匹配规则后转发到一个文件，同时url修改为访问文件的url路径。
   目的文件路径的根目录(root)是fis本地调试目录(可以输入命令 $ fis server open 打开噢)，配置文件从根目录之后写即可。
   其中，转发到php文件，php会执行。转发到其他文件，会返回文件内容。例如：可以用rewrite模拟ajax请求，返回json数据~
4. rewrite的目的文件，默认需要经过fis release之后投送到fis调试目录。直接转发到本地文件，该文件可能没有被fis处理，可能会出现诡异问题噢~
5. **template** :  为了完全模拟线上url，可通过template配置对应的url规则对应相应的模板进行访问，访问url不变。
6. **redirect** ： 匹配规则后重定向到另一个url。

## 注意
1.   server.conf专门配置转发规则，文件名不能改哦
2.   项目很大，多模块时，一个模块配置server.conf就可以啦，比如在common模块配置全站的转发规则，否则会覆盖
3.   所有非rewrite、template和redirect开头的会被当做注释处理。

## 高级使用

fis默认的rewrite，redirect转发无法满足你的需求怎么办？别怕，fis允许用户自定义全新的规则噢~ 自定义的规则优先级最高，会在server.conf保存的规则之前匹配url

### addRewriteRule
使用：rewrite提供 public static function addRewriteRule($reg, callable $callback){}公共静态方法。
用户可以在自己的处理url的php文件中添加自己的url处理规则。

### 参数
1. $reg 匹配url的正则，注意正则需要添加定界符。例如：“/^\/photo\/?$/”

2. $callback 匹配到$reg之后，处理匹配的回调函数。这个函数有两个参数($matches, $root)
   $matches 匹配$reg后的匹配数组
   $root是根目录。默认是fis本地的调试目录，用户可以通过rewrite的公共方法设置。
   public static function setRoot($root){}