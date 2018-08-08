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

2，操作Object属性的方法
----

    var myObject = {
        a: 2
    }
    Object.getOwnPropertyDescriptor(myObject, "a")   // 获取对象的属性描述符
    Object.defineProperty(myObject, "a", {      // 添加或修改一个新属性
        value: 3,
        writable: true,         // 决定属性是否可以修改
        configurable: true,     //  决定属性是否可配置 ---false--- 除了writable和value其他配置都不可修改。
                                // writable可以单向改为false，它决定value是否可改
                                //  还会禁止删除这个属性
        enumerable: true        //  决定在for...in中是否可枚举
    })
    
定义一个*不可修改、重定义和删除*的常量：`writable:false`和`configurable:false`      
还禁止添加新属性：`Object.preventExtensions(myObject)`     

`Object.seal()`（密封）等同于`Object.preventExtensions(myObject)` +`configurable:false`

`Object.freeze()`（冻结）等同于`Object.seal()`+ `writable:false`

3,Getter和Setter
-----
get和set也属于属性描述符

    var myObject = {
        a: 2
    }
    
myObject.a是一次属性访问，实际上是实现了`[[Get]]`操作。       
* 首先在对象中查找是否有名称相同的属性，有就返回这个属性的值；没有找到则遍历可能存在的`[[Prototype]]`(原型)链

`setter`ui覆盖单个属性默认的`[[Put]]`(赋值)操作。

4,存在性
------
`myObject.c`的属性返回值为undefined----可能是属性`c`中储存的就是undefined，也可能`c`属性并不在`myObject`中。
 
 如何判断对象中是否存在某个属性？
 
 * `in`操作符：`("a" in myObject)` 会检查属性是否在对象及其`[[Prototype]]`原型链中。         

    *`in`操作符实际上检查的是属性名是否存在，所以`4 in [2, 4, 6]`的结果为`false`*
 
 
 * `hasOwnProperty`:`myObject.hasOwnProperty("a")` 会检查属性是否在`myObject`中，不会检查`[[Prototype]]`链。        
    
**PS** ：*通过方式`Object.create(null)`创建的对象没有连接到Object.prototype，此时`myObject.hasOwnPrototype()`就会失败*
-----这时就需要使用更强硬的方法----`Object.prototype.hasOwnProperty.call(myObject, "a")`
    
5,枚举
----
`enumerable：false`只会控制属性不在`for...in`中出现，该属性还是可以正常获取。

`myObject.propertyIsEnumerable("..")`：检查属性名是否存在于对象中（不检查原型链），并且满足`enumerable:true`。
`Object.keys(..)`：返回所有可枚举属性的数组（不检查原型链）


6,遍历
-----
`for...in`：遍历可枚举属性名
* 遍历对象属性的顺序是不确定的，不同的JavaScript引擎中可能不一样。     

`for..of`：遍历属性值
* 通过调用被访问对象的迭代器对象的`next()`方法来遍历所有返回值。

使用数组内置的`@@iterator`来手动遍历数组：

    var myArray = [1, 2, 3 ];
    var it = myArray[Symbol.iterator]();
    
    it.next();  // {value: 1,done: false}
    it.next();  // {value: 1,done: false}
    it.next();  // {value: 1,done: false}
    it.next();  // {value: 1,done: false}
    it.next();  // {done: true}
    
还有部分**给对象自定义迭代器**的代码这里就不记录了