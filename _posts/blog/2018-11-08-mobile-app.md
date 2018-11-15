---
layout: post
title: 手机端开发遇到过的问题记录
categories: ios
description: 记得几个写几个
keywords: python, chrome, selenuim
---

整理一下自己遇到过的几个手机端遇到的问题

1，IOS--点击列表时会出现阴影
----
解决办法：
在出现阴影的部分加上样式`-webkit-tap-highlight-color:rgba(255,0,0,0.5)`

2，IOS--iphone不支持input框的文件选择功能
-----
主要是在iphone上面，可能会不支持`input type=‘file’`的标签

3，IOS--禁止事件冒泡
-----
在form表单中要禁止事件冒泡,否则点击一次input表单就会提交一次

4，IOS--滑动卡顿的问题
------
给容器元素加上属性` -webkit-overflow-scrolling: touch;`