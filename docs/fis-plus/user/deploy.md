---
layout: post
title: fis-plus 部署上线
category: fis-plus/user
---

## 上传测试机

> fis 通过简单的上传配置，把编译后文件上传到测试机,方便联调。

* [产出目录](./deploy-test.html#产出目录)
* [上传配置](./deploy-test.html#上传配置)
* [上传常见问题](./deploy-test.html#上传常见问题)

## 发布上线

> 提供基础的build.sh编译脚本，用于上线时候自动编译项目。

```bash
    #!/bin/bash

    MOD_NAME="output"
    TAR="$MOD_NAME.tar.gz"

    # add path
    export PATH=/home/fis/npm/bin:$PATH
    #show fis-plus version
    fisp --version --no-color

    #通过fis-plus release 命令进行模块编译 开启optimize、md5、打包功能，同时需开启-u 独立缓存编译方式，产出到同目录下output中
    fisp release -cuompd output
    #进入output目录
    cd output
    #删除产出的test目录
    rm -rf test

    #将output目录进行打包
    tar zcf $TAR ./*
    mv $TAR ../

    cd ..
    rm -rf output

    mkdir output

    mv $TAR output/

    echo "build end"
```
