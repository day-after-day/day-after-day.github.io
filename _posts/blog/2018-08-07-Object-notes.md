---
layout: post    
title: 对象初始化        
categories: Blog        
description: 记录JavaScript中对象的相关知识点进。     
keywords:  object        
---

1，类型
----
JavaScript主要有七种基本类型：string, number, boolean, null,undefined, object, Symbol(ES6)         

简单基本类型：string, number, boolean, null, undefined, symbol         
* 简单基本类型本身并不是对象----“JavaScript中万物皆对象”显然是错的。 

`typeof null` 有时会返回`"object"`，这其实是语言本身的一个BUG。       
实际上null本身是基本类型。---- 原理是这样：不同的对象再底层都表示为二进制，再JavaScript中二进制前三位都为0的话会被判断为object类型。
由于null的二进制表示全是0，自然前三位也是0，所以执行typeof时会返回`"object"`。


内置对象：String, Number, Boolean, Object, Function, Array, RegExp, Date, Error        
特点：将这些内置函数当作构造函数来实用，从而**构造一个对应子类型的新对象**。      
     
* 原始值 `"i am a string ` 并不是一个对象，他只是一个字面量，并且是一个不可变的值。
如果要在这个字面量上执行一些操作----获取长度，访问其中某个字符等----那就需要将其转换为String对象。
幸好，在必要时语言会自动把字符串字面量转换成一个String对象。

* null和undefined没有对应的构造形式，他们只有文字形式。相反，Date只有构造形式，没有文字形式。

2，内容
----