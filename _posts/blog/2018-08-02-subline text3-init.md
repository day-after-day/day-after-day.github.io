---
layout: post    
title: sublime初始化        
categories: Blog        
description: 记录几款实用的sublime插件包。     
keywords:  sublime, package        
---

sublime text3初始化配置
====

1,如何安装插件
----
* Ctrl+Shift+P调出命令面板 
* 输入install 调出 Install Package 选项并回车        
	 如果直接出现package列表，则可开始下一步；       		
     若原有的列表 消失，则重复上述操作，列表即可出现。
* 接下来输入需要的插件名称，然后选择即可安装~


2,前端必备铁三角
----
* Emmet
Emmet 快速生成HTML代码。 附带css提示功能
例如`!+Tab键`初始化最基础的html，以及`div.box+Tab键`快速生成class名为box的div等实用功能。

相关文档：
[sublime text3---Emmet：HTML/CSS代码快速编写神器](http://www.cnblogs.com/EnSnail/p/6294897.html)
[Emmet — the essential toolkit for web-developers](https://docs.emmet.io/)

* sublimeCodeIntel
sublimeCodeIntel ：js代码提示工具

•DocBlockr      	
DocBlocker ：一款自动补全注释插件，支持众多语言

3,vue项目开发相关插件
----
* Vue Syntax Highlight
Vue高亮插件，它不仅可以使代码高亮起来，还能进行代码智能提示。

*Syntax Highlighting for Sass
保持<style lang="scss" scoped>中的scss语法高亮

* sass
sass/scss语法提示工具

4，其他实用插件
------
上面只介绍了开发需要用的最基础插件，但是还有很多好用的插件能提高我们的敲代码，检查速率。

* BracketHighlighter
高亮显示[], (), {}, “”, ”, <tag></tag>符号，便于查看起始和结束标记。

* ColorPicker
调色板，需要输入颜色时，可直接选取颜色。使用快捷键ctrl+shift+c即可打开调色板
（ps:若打不开调色板，可能是快捷键冲突导致。需要前往`Preferences→Key Bindings-User`中配置）

* AutoFileName
文件路径提示

* Sidebar Enhancements
增强侧栏文件右键功能，比如复制文件路径

* CSS Format
整理scss/css样式，代码格式化为展开、紧凑、压缩多种可选的形式
ps:对于.vue文件无法生效

* Trailing spaces
检测并且高亮显示 和 一键删除代码的空格，保存时自动删除多余空格.
功能入口：edit→Trailing Spaces→Delete


5,markdown相关插件
----
•Markdown Editing       
Markdown Editing并不只是一个markdown的主题插件		
它自定义许多markdown的快捷键,例如ctrl+2是二级标题,还有许多可以看配置文件和项目的[github主页](https://github.com/SublimeText-Markdown/MarkdownEditing)		
推荐配置

		"color_scheme": "Packages/Boxy Theme/schemes/Boxy Monokai.tmTheme", // 修改风格的主题,我这里是sublime的boxy主题自带的,默认有这几种主题 
        //"color_scheme": "Packages/MarkdownEditing/MarkdownEditor.tmTheme", 
        // "color_scheme": "Packages/MarkdownEditing/MarkdownEditor-Dark.tmTheme",
        // "color_scheme": "Packages/MarkdownEditing/MarkdownEditor-Yellow.tmTheme", 
        "highlight_line": true, // 高亮正在编辑的行 
        "line_numbers": true, // 显示行号 
        "tab_size": 4, // tab宽度 
        "translate_tabs_to_spaces": true, // tab转换为空格 
        "trim_trailing_white_space_on_save": true, // 保存时去掉行尾空格 
        "word_wrap": true, // 自动换行 
        "wrap_width": "auto", // 换行的宽度,默认80会造成左侧大量留白 
        "mde.keep_centered": true, // 可以保持你正在编辑的行始终处于屏幕的中间


•MarkdownLivePreview        	
MarkdownLivePreview可以实现实时预览		
在首选项->Package Setting里修改MarkdownLivePreview的user配置文件,设置在打开时同步预览

	"markdown_live_preview_on_open": true

4,热键冲突
----
ctrl+\`  ---- sublime呼出控制台的快捷键，与*搜狗浏览器*的老板键：ctrl+~ 冲突



5,BUG解决
-----
报错信息：
[sublime there are no packages](https://www.cnblogs.com/fayin/p/6414735.html)

6,相关介绍文章：
------
[https://blog.csdn.net/huohao_blogs/article/details/76120756](https://blog.csdn.net/huohao_blogs/article/details/76120756)


