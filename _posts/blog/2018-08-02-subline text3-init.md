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
快速生成HTML代码。 附带css提示功能
例如`!+Tab键`初始化最基础的html，以及`div.box+Tab键`快速生成class名为box的div等实用功能。

相关文档：
[sublime text3---Emmet：HTML/CSS代码快速编写神器](http://www.cnblogs.com/EnSnail/p/6294897.html)
[Emmet — the essential toolkit for web-developers](https://docs.emmet.io/)

* sublimeCodeIntel
js代码提示工具

* DocBlockr      	
一款自动补全注释插件，支持众多语言

* sublimeTmpl
文件模板插件      
[创建vue模板](https://www.jianshu.com/p/54f7a2f9b30d)

3,vue项目开发相关插件
----
* Vue Syntax Highlight
Vue高亮插件，它不仅可以使代码高亮起来，还能进行代码智能提示。

* Syntax Highlighting for Sass
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

/** Sidebar Enhancements
(此插件会导致原来的右键功能`Open Containing Folder`丧失，而且删除文件还会增加一步*确认删除*，不建议安装)
增强侧栏文件右键功能，比如复制文件路径
配置：

    [
        { "keys": ["ctrl+shift+c"], "command": "copy_path" },
        //chrome
        { "keys": ["f2"], "command": "side_bar_files_open_with",
                "args": {
                    "paths": [],
                    "application": "C:\\Users\\jeffj\\AppData\\Local\\Google\\Chrome\\Application\\chrome.exe",
                    "extensions":".*"
                }
         }
    ]*/

* CSS Format
整理scss/css样式，代码格式化为展开、紧凑、压缩多种可选的形式
ps:对于.vue文件无法生效

* Trailing spaces
检测并且高亮显示 和 一键删除代码的空格，保存时自动删除多余空格.
功能入口：edit→Trailing Spaces→Delete

5,terminal插件
------
在markdown中，打开一个当前位置的新窗口     
先下载cmder：
[cmder](https://github-production-release-asset-2e65be.s3.amazonaws.com/11276147/b222f850-32b5-11e4-873b-f061698faec1?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20181115%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20181115T070131Z&X-Amz-Expires=300&X-Amz-Signature=edb332c31d980457b4e5d456f289ef403823b48fd86ff0a5549b3efb0ed78f7c&X-Amz-SignedHeaders=host&actor_id=26592626&response-content-disposition=attachment%3B%20filename%3Dcmder.zip&response-content-type=application%2Foctet-stream)
[cmder_mini](https://github-production-release-asset-2e65be.s3.amazonaws.com/11276147/b23fdb3c-32b5-11e4-8166-5d2f2bc3b251?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20181115%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20181115T070123Z&X-Amz-Expires=300&X-Amz-Signature=429fec363033d4fec5deab16ed5ce68dc8d38e32f59e1ea922005c5a17b746f2&X-Amz-SignedHeaders=host&actor_id=26592626&response-content-disposition=attachment%3B%20filename%3Dcmder_mini.zip&response-content-type=application%2Foctet-stream)

配置：`Preferences > Package setting > Terminal > Setting -User`

    {
      // window下终端路径
      // "terminal": "F:\\cmder_mini\\Cmder.exe",
      "terminal": "F:\\cmder\\Cmder.exe",
      //  window下终端参数
      "parameters": ["/START", "%CWD%"]
    }

默认快捷键：      
`ctrl+shift+t` 打开文件所在路径的cmder窗口


6,markdown插件
----
* Markdown Editing       
Markdown Editing并不只是一个markdown的主题插件		
它自定义许多markdown的快捷键,例如ctrl+2是二级标题,还有许多可以看配置文件和项目的[github主页](https://github.com/SublimeText-Markdown/MarkdownEditing)		



* MarkdownPreview        	
Mardown Preview支持在浏览器中预览markdown文件，还支持三种预览方式：github(在线),gitlab(在线),markdown（本地）。       
使用：`ctrl+shift+p`, 输入`markdown previewer`/`mdp`，回车,选择一种预览方式。
设置快捷键:` { "keys": ["alt+m"], "command": "markdown_preview", "args": {"target": "browser", "parser":"markdown"}  }`

7,热键冲突
----
ctrl+\`  ---- sublime呼出控制台的快捷键，与*搜狗浏览器*的老板键：ctrl+~ 冲突


8,BUG解决
-----
报错信息：
[sublime there are no packages](https://www.cnblogs.com/fayin/p/6414735.html)

9,相关介绍文章：
------
[https://blog.csdn.net/huohao_blogs/article/details/76120756](https://blog.csdn.net/huohao_blogs/article/details/76120756)


