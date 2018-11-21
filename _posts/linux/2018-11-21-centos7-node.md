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

打开控制台，输入`wget https://nodejs.org/dist/v6.10.1/node-v6.10.1-linux-x64.tar.xz `

2，将下载文件解压缩
-----
对于.xz后缀结尾的node包,输入：

    xz -d node-v6.10.1-linux-x64.tar.xz

此时包被解压缩成了`node-v6.10.1-linux-x64.tar`，继续解压

    tar -xf node-v6.10.1-linux-x64.tar

文件夹改名:

    mv node-v6.10.1-linux-x64 node-v6.10.1

3，验证是否能够使用node
-----

    cd node-v6.10.1/bin
    ./node -v  // 查看node版本  
    // v6.10.1

4，设置node和npm指令到全局
----

    ln -s ~/node-v9.3.0-linux-x64/bin/node /usr/bin/node
    ln -s ~/node-v9.3.0-linux-x64/bin/npm /usr/bin/npm

5，测试
----

    node -v
    npm -v


6,相关文章
----
[CentOS安装NodeJS](https://blog.csdn.net/xerysherryx/article/details/78920978)
