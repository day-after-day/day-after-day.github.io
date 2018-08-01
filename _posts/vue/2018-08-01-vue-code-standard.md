---
layout: post        
title: vue 项目开发编码规范     
categories: vue     
description: 记录下公司vue开发中常遵循的一些规范        
keywords: Vue, standard        
---

整理记录一下过往vue项目中约定的一些规范

1,图片的引入
----
使用图片url时，只允许使用绝对路径(~@)的形式。      
* 如果使用相对路径，则可能会在打包时找不到对应的文件。        
而且绝对路径书写更直观方便，即使scss文件变换位置，图片地址也不受影响。

2,组件的命名规范
----
在注册组件时，使用首字母大写的驼峰命名法（camelCased），       
在使用组件时，使用短横线隔开式(kebab-case)，且不允许使用闭合标签的形式。  
* 一个vue的组件，其实可以认为是一个类，类的命名一般用大写字母开头。


3,标签内的逻辑
----
除了简单的逻辑，用三元运算符写在标签内以外，其他复杂的js逻辑不允许直接写在标签内
* 禁止将函数直接写在标签上！


4,避免使用table布局
----
展示列表信息时，尽量避免使用table布局，用ul>li布局替换。
* table布局只在展示简单信息，页面结构比较简单时使用。      
    在td内部的布局稍微复杂一点时，向其内部添加新的结构将会变得越来越困难。
    
    
5,事件挂载
----
如非必要，不建议直接修改body和document，或者在上面添加事件监听;      
单页面逻辑中，挂在window上的事件，一定要记得*在组件销毁时移除！*     

这些都是会对全局产生影响的操作，要慎重！
    
    
6，常量集中定义
----
将各类全局需要使用的常量，集中定义在contents文件夹内的js文件中，方便日后的修改维护。


7,跨域请求集中定义
----
[跨域请求集中定义](../blog/2018-08-01-reset-axios.md)

8,scss的使用
----
* 将常用的样式用`@mixin`统一定义起来，配合插件`sass-resources-loader`，达到任意地方都可以直接引用的目的。

* 非公共的scss样式，需要引入到对应的.vue文件中使用，而不允许直接引入公共文件main.js中。        

      <style lang="scss" scope>
         @import "~@/scss/login.scss"
      <style>  
      
    好处是：减少初次加载的css体积；各个组件间的样式不会相互影响，命名更方便；
    