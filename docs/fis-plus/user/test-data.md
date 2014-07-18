---
layout: post
title: 测试数据
category: fis-plus/user
---

> FIS提供了本地调试数据功能，本地预览模板时会根据选择的测试数据进行展现，默认的smarty版本为3.1.13。

### 浏览器书签

```javascript

//新建浏览器书签，网址为以下内容
javascript: void function() {var d = new Date();d.setFullYear(d.getFullYear() + 1);document.cookie='FIS_DEBUG_DATA=4f10e208f47bfb4d35a5e6f115a6df1a;path=/;expires=' + d.toGMTString() + '';location.reload(); }();

```

当模块进行编译发布后，在预览的时候点击书签，进入数据管理页面，修改数据后再进行渲染:
![fisp test](http://c.hiphotos.bdimg.com/album/s%3D550%3Bq%3D90%3Bc%3Dxiangce%2C100%2C100/sign=83ad2515d1a20cf44290feda46323a0b/b3b7d0a20cf431ad11c0e1ab4a36acaf2edd9816.jpg?referer=e006ba57e1fe9925921b5c60d80c&x=.jpg)

修改保存的数据会临时保存在发布目录中，如果需要保存在test目录下还需要单独粘贴复制。

### 测试数据
测试数据存放在模块test目录下，模板文件和测试数据文件对应关系如下：

模板:
    `photo/page/index.tpl`

对应数据文件:

    0. photo/test/page/index.php (php格式)
    0. photo/test/page/index.json (json格式)

也支持多份数据(php格式为例):

    0. photo/test/page/index/index_1.php
    0. photo/test/page/index/index_2.php
    ...

文件名: `index_\d+.php`