---
layout: post
title: 上传测试机
category: fis-plus/user
---

<!--
title: 上传部署
toc: {:toc}
tpl: detail
-->

## 产出目录

FIS 根据目录规范默认设置了文件的产出路径：

```bash
└── config
│    └── modulename-map.json    静态资源表
├── template
│    ├──home
│    │   └── page
│    │         └── index.tpl      page级模板文件
│    │   └── widget
│    │         └── section
│    │           └── section.tpl   widget模板文件
├── static
│    └── home
│    │   ├── pkg
│    │   │   ├── demo.css  打包css文件
│    │   │   └── demo.js   打包js文件
│    │   ├── index
│    │   │   ├── index.css  page级css文件
│    │   │   └── index.js   page级js文件
│    │   └── widget             widget组件目录
│    │       └── section
│    │               └── section.css
├── plugin
├── test
```

用户根据产出后的目录，只需要将 **config、template、static、plugin** 目录上传至联调机器进行项目联调。

## 上传配置

用户通过配置 **deploy节点** 进行文件上传，fis支持使用post请求向http服务器发送文件，服务器端可以使用php、java等后端逻辑进行接收，[fis-command-release](https://github.com/fis-dev/fis-command-release)插件中提供了一个这样的 [php版示例](https://github.com/fis-dev/fis-command-release/blob/master/tools/receiver.php)，用户可以直接部署此文件于接收端服务器上。

``注意： receiver.php放到测试机后，请在浏览器中直接访问该文件，保证可以访问。
可以访问的情况下，页面会显示 I'm ready for that, you know. ``

根据产出目录以及联调机器，进行上传配置：

### 上传到1个测试机

```javascript
//上传到一个 remote测试机
fis.config.set('deploy', {
    //使用fis release --dest static来使用这个配置
    remote: [{
        //如果配置了receiver，fis会把文件逐个post到接收端上
        receiver : 'http://www.example.com/path/to/receiver.php',
        //从产出的结果的static目录下找文件
        from : '/static',
        //上传目录从static下一级开始不包括static目录
        subOnly : true,
        //保存到远端机器的/home/fis/www/static目录下
        //这个参数会跟随post请求一起发送
        to : '/home/fis/www/',
        //某些后缀的文件不进行上传
        exclude : /.*\.(?:svn|cvs|tar|rar|psd).*/
    },
    {
        //如果配置了receiver，fis会把文件逐个post到接收端上
        receiver : 'http://www.example.com/path/to/receiver.php',
        //从产出的结果的config目录下找文件
        from : '/config',
        //保存到远端机器的/home/fis/www/config目录下
        //这个参数会跟随post请求一起发送
        to : '/home/fis/www/',
        //某些后缀的文件不进行上传
        exclude : /.*\.(?:svn|cvs|tar|rar|psd).*/
    },
    {
        //如果配置了receiver，fis会把文件逐个post到接收端上
        receiver : 'http://www.example.com/path/to/receiver.php',
        //从产出的结果的template目录下找文件
        from : '/config',
        //保存到远端机器的/home/fis/www/template目录下
        //这个参数会跟随post请求一起发送
        to : '/home/fis/www/',
        //某些后缀的文件不进行上传
        exclude : /.*\.(?:svn|cvs|tar|rar|psd).*/
    },
    {
        //如果配置了receiver，fis会把文件逐个post到接收端上
        receiver : 'http://www.example.com/path/to/receiver.php',
        //从产出的结果的plugin目录下找文件
        from : '/plugin',
        //保存到远端机器的/home/fis/www/plugin目录下
        //这个参数会跟随post请求一起发送
        to : '/home/fis/www/',
        //某些后缀的文件不进行上传
        exclude : /.*\.(?:svn|cvs|tar|rar|psd).*/
    }]
});
```

### 上传到多个测试机

```javascript
//上传到remote，local，remote2三台机器
//fis-conf.js
fis.config.set('deploy', {
    //使用fis release --dest remote来使用这个配置
    remote : {
        //如果配置了receiver，fis会把文件逐个post到接收端上
        receiver : 'http://www.example.com/path/to/receiver.php',
        //从产出的结果的static目录下找文件
        from : '/static',
        //保存到远端机器的/home/fis/www/static目录下
        //这个参数会跟随post请求一起发送
        to : '/home/fis/www/',
        //通配或正则过滤文件，表示只上传所有的js文件
        include : '**.js',
        //widget目录下的那些文件就不要发布了
        exclude : /\/widget\//i,
        //支持对文件进行字符串替换
        replace : {
            from : 'http://www.online.com',
            to : 'http://www.offline.com'
        }
    },
    //名字随便取的，没有特殊含义
    local : {
        //from参数省略，表示从发布后的根目录开始上传
        //发布到当前项目的上一级的output目录中
        to : '../output'
    },
    //也可以是一个数组
    remote2 : [
        {
            //将static目录上传到/home/fis/www/webroot下
            //上传文件路径为/home/fis/www/webroot/static/xxxx
            receiver : 'http://www.example.com/path/to/receiver.php',
            from : '/static',
            to : '/home/fis/www/webroot'
        },
        {
            //将template目录内的文件（不包括template一级）
            //上传到/home/fis/www/tpl下
            //上传文件路径为/home/fis/www/tpl/xxxx
            receiver : 'http://www.example.com/path/to/receiver.php',
            //from支持正则
            from : '/template',
            to : '/home/fis/www/tpl',
            //subOnly仅上传
            subOnly : true
        }
    ]
});
```
### 小贴士

* **--dest参数** 支持使用逗号（,）分割多个发布配置，比如上面的例子，我们可以使用fis release --dest **remote,plugin** 命令在一次编译中同时发布多个目标。

* ``subOnly参数``
默认上传from整个目录到测试机。添加subOnly参数仅上传from目录下文件。

* replace替换多个字符串
需要replace替换多个字符串，可以使用正则的方式。例如：

```javascript
replace : {
    from : /www\.a\.com|www\.b\.com/,
    to : function(m){
        if(m === 'www.a.com') return 'www.x.com';
        if(m === 'www.b.com') return 'www.y.com';
    }
}
```

## 上传常见问题

1. ``上传到一半突然中断,怎么办？``

    A: 请关闭测试机的服务器的防火墙，然后重新上传。至于服务器防火墙怎么关闭，不同服务器不一样，所以还是查文档或者google一下吧