---
layout: post
title: vue项目结构
categories: vue
description: vue项目结构。
keywords:  vue, vue-cli
---

基于vue-cli搭建的vue项目，脚手架版本为2.*

1，代码目录划分
----
目录结构设定原则：
1，逻辑与业务分离
将逻辑与业务分离，提高可阅读性、复用性，文件结构清晰，更利于维护和长期开发。
2，目录下必定有一个文件名为.js/.scss/vue的文件，作为该目录的主要内容


	目录/文件           说明
	src	
     |
     |--assets	 资源目录 assets 用于组织未编译的静态资源,如 LESS、SASS 或 JavaScript。
     |   |
     |   |--img		存放图片（图片资源放在static下或者此处二选一）
     |   |  
     |   |--js		
     |   |  |--base		存放公共的与业务逻辑无关的js函数，cookie，getStyle,deepCopy
     |   |  |--axios  	存放axios再封装函数，默认设置请求方式，超时时间，拦截器等
     |   |  |--api		集中定义后台接口
     |   |  |--common	存放与业务逻辑相关的公共函数
     |   |  |--data		存放前端定义的常量，用于全局引用
     |   |  |--reg		存放正则表达式
     |   |  
     |   |--scss
     |   |  |--base		存放与业务逻辑无关的scss函数，mixins和变量
     |   |  |--common	存放样式重置表，element-ui默认样式修改，等全局性公共样式，
     |   |  |--pages	与各个页面对应的scss样式；样式较多，或多个页面公用同一分样式时使用
     |   |  
     |--components		 组件目录 components 用于组织应用的 Vue.js 组件以及方法和指令
     |   |  
     |   |--base		存放与业务逻辑无关的vue组件，如弹出层，展开，上拉下拉等
     |   |  |--index.js	import所有的组件，并且export导出，方便项目使用
     |   |  
     |   |--common		存放与项目有紧密联系的业务组件
     |   |--directives	存放自定义指令
     |   |--methods		存放vue上的自定义方法
     |   |  
     |--pages	页面目录 pages 用于组织应用的路由及视图
     |--router	路由目录router用于联系vue页面，设定路由
     |--store	store 目录用于组织应用的 Vuex 状态树 文件
     |   |--index.js
     |   |--actions.js
     |   |--mutations.js
     |   |--getters.js
     |   |--states.js
     |
     |
     static		资源目录static用户存放静态资源，放于此目录下的文件会被直接复制到目标文件中
     |
     |--plugins	用于存放已经编译好的js插件
     |--sources	用于存放直接使用的静态资源，如图片，视频等
     |     |--img
     |     |--video
     



打包后的文件结构(自动生成)

	dist
     |
     |--index.html
     |
     |--static
     |   |--css
     |   |--img
     |   |--js
     |   |--fonts
     |   |--plugins
     |   |--sources