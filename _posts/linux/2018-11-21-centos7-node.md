---
layout: post
title: centos7环境下安装node
categories: linux
description: centos7环境下安装node
keywords: linux, centos7, node
---

centos7环境下安装node

1，获取node安装包下载地址
----
进入[node官网下载地址](https://nodejs.org/en/download/),复制目标链接

安装wget：`yum install -y wget`

打开控制台，输入`wget https://nodejs.org/dist/v10.13.0/node-v10.13.0-linux-x64.tar.xz`

2，将下载文件解压缩
-----
对于.xz后缀结尾的node包,输入：

    xz -d node-v10.13.0-linux-x64.tar.xz

此时包被解压缩成了`node-v10.13.0-linux-x64.tar`，继续解压

    tar -xf node-v10.13.0-linux-x64.tar

文件夹改名:

    mv node-v10.13.0-linux-x64 node-v10.13.0

3，验证是否能够使用node
-----

    cd node-v10.13.0/bin
    ./node -v  // 查看node版本  
    // v10.13.0

4，设置node和npm指令到全局
----

    ln -s ~/node-v10.13.0/bin/node /usr/bin/node
    ln -s ~/node-v10.13.0/bin/npm /usr/bin/npm

5，测试
----

    node -v
    npm -v

6,BUG报错
-----
* 对于`ln: failed to create symbolic link ‘/usr/bin/node’: Permission denied`：用户权限不够，切换为root账户，再来操作。

* 对于`ln: failed to create symbolic link ‘/usr/bin/node’: File exists`：说明已经软链接已经创建好了。

7，配置全局环境变量
----
目标：使用`npm install -g xxxx`后可以在全局使用`xxxx`插件

修改配置文件:

    vi /etc/profile

移动到最后一行,新增2行:

    export NODE_HOME=/usr/local/node_global
    export PATH=${NODE_HOME}/bin:$PATH

移动到：

    cd /usr/local/
    npm config set prefix "node_global"
    npm config set cache "node_cache"
    source /etc/profile

完成，可以愉快的使用`npm install -g xxxx`全局安装插件了。

7,相关文章
----
[CentOS安装NodeJS](https://blog.csdn.net/xerysherryx/article/details/78920978)

[Centos 配置nodejs&npm 全局环境变量](https://blog.csdn.net/qingaoti/article/details/80536933)