---
layout: post
title: centos7环境下安装nginx
categories: linux
description: centos7环境下安装nginx
keywords: linux, centos7, nginx
---

centos7环境下安装nginx

1,安装所需环境
----
nginx编译依赖gcc环境

    yum install gcc-c++

nginx 的 http 模块使用 pcre 来解析正则表达式，所以需要在 linux 上安装 pcre 库;pcre-devel 是使用 pcre 开发的一个二次开发库。nginx也需要此库

    yum install -y pcre pcre-devel     

 nginx 使用 zlib 对 http 包的内容进行 gzip ，所以需要在 Centos 上安装 zlib 库
 
    yum install -y zlib zlib-devel

nginx 不仅支持 http 协议，还支持 https（即在ssl协议上传输http），所以需要在 Centos 安装 OpenSSL 库。
* OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及 SSL 协议，并提供丰富的应用程序供测试或其它目的使用。

    yum install -y openssl openssl-devel

2,nginx下载安装
----
直接下载.tar.gz安装包，地址：[https://nginx.org/en/download.html](https://nginx.org/en/download.html)

    wget -c https://nginx.org/download/nginx-1.15.11.tar.gz

解压

    tar -zxvf nginx-1.15.11.tar.gz

使用默认配置

    cd nginx-1.15.11
    ./configure --with-http_ssl_module

编译安装

    make && make install

查找nginx安装路径

    whereis nginx    // /usr/local/nginx

    
进入nginx安装目录

    cd /usr/local/nginx
    cd sbin

启动nginx

    ./nginx

查询nginx进程

    ps aux|grep nginx

此时输入**当前IP地址**，就可以看到nginx页面了。
![](/images/linux/nginx-homepage.png)


3,实用配置
-----
开机自启动

    vi /etc/rc.local

在最后添加一行

    /usr/local/nginx/sbin/nginx

(可选)设置执行权限：

    chmod 755 rc.local

4,其他操作
----    
**在对应目录下**

停止nginx

    ./nginx -s quit:此方式停止步骤是待nginx进程处理任务完毕进行停止。
    ./nginx -s stop:此方式相当于先查出nginx进程id再使用kill命令强制杀掉进程。

重启nginx 

    ./nginx -s quit
    ./nginx

重新加载配置文件：     
当 ngin x的配置文件 nginx.conf 修改后，要想让配置生效需要重启 nginx; 使用`-s reload`不用先停止 ngin x再启动 nginx 即可将配置信息在 nginx 中生效

    ./nginx -s reload


5，参考文章
----
[centos7安装Nginx](https://www.cnblogs.com/kaid/p/7640723.html)