---
layout: post
title: centos7常用操作指令
categories: linux
description: centos7常用操作指令
keywords: linux, centos7
---

centos7常用操作指令

1,删除
----

    rm a    
    rm -r a
    rm -rf a // 递归删除文件夹及其内容

2，移动/改名
----

    mv a b      // 将a改名成b
    mv a/c b    // 将a/c移动到b文件夹下

3，查看文件
----
cat 命令的原含义为连接(concatenate)， 用于连接多个文件内容并输出到标准输出流中(标准输出流默认为屏幕)。实际运用过程中，我们常使用它来显示文件内容。
    
    cat file1.php