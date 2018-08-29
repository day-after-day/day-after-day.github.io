---
layout: post
title: Chrome浏览器下的input框设置
categories: Blog
description: Chrome浏览器下的input框设置。
keywords:  chrome, input
---



chrome下的input框设置
------
需求：设置input框背景为灰黑色，字体为灰白色。
环境：less

	@bg:#22212f;
    @color:#6a7581

设置input背景色（默认为黄色）

	 -webkit-box-shadow: 0 0 0 1000px @bg inset !important;
     

设置自动填充字体颜色

	text-fill-color: @color;
	-webkit-text-fill-color: @color; 
    
设置placeholder字体颜色

	input:-ms-input-placeholder{
        color: @color;
    }

    input::-webkit-input-placeholder{
        color: @color;
    }
    
    
去除 input 焦点 边框

	 outline:none;
     