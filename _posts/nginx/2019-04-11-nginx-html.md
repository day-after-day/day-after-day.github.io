---
layout: post
title: 网站http升级为https
categories: nginx
description: 网站http升级为https
keywords: nginx, centos7
---

按照[之前的教程](http://gaofangshang.com//2018/11/22/centos7-nginx/)，安装完nginx后。


1，进入nginx的位置
----
cd /usr/local/nginx

2，将网站证书放在服务器上
-----
mkdir cert

然后讲证书放在cert文件夹内


2，编辑配置文件
-----
vim /usr/local/nginx/conf/nginx.conf

####配置https
* 找到https对应的server （listen为443 ssl），解除掉原来的注释。
* 将ssl_certificate后面的值修改为 ` /usr/local/nginx/cert/你的证书名称.pem;`
* 将 ssl_certificate_key 后面的值修改为 ` /usr/local/nginx/cert/你的证书名称.key;`

ok.(如果你此时想确定是否修改成功，可以退出编辑，重启nginx `./usr/local/nginx/sbin/nginx -s reload` ，
然后在网站上输入 `htts://你的网站地址`, 查看到网页。)


此时http和https并存，所以**需要强制将http重定向为https**

配置文件需要修改：
* server(listen为80) 下，server_name 后面的值，不管原来为`_ `或者是`localhost`全都改为*你网站的地址*
  比如 `baidu.com ` 前面不带http，www之类的操作。
* 在server_name下面一行添加  `return      301 https://$server_name$request_uri;`

* 退出编辑，重启nginx: `./usr/local/nginx/sbin/nginx -s reload` 

此时在浏览器输入  你的网站地址，就会自动进入https页面了。


