﻿﻿﻿﻿﻿---layout: posttitle: 强制类型转化categories: Blogdescription: 强制类型转化keywords:  ES5---

强制类型转化

1,JSON.string
-----
所有*安全的JSON值*都可以使用`JSON.stringify(...)`字符串化。

`JSON.stringify(..)` 在对象中遇到undefined、function和symbol时会自动将其忽略，在数组中则会返回null(以保证单元位置不变)

如果对象中定义了`toJSON()`方法，JSON字符串化时会首先调用该方法，然后用它的返回值来进行序列化。

我们可以向JSON.stringify(..)传递一个可选参数replacer。      
如果replacer是一个数组，那么它必须是一个字符串数组，其中包含序列化要处理的对象属性名称；
如果replace是一个函数，那么它会对对象本身调用一次，然后对对象中的每个属性各调用一次，每次传递两个参数，键和值。如果要忽略每个键就返回undefined，否则返回指定的值；

JSON.stringify还有一个可选参数space，用来指定输出的缩进格式。可为正整数，字符串。eg:`JSON.stringify(a, null, 3)`

2,date
----

    // 获取当前的时间戳
    // 隐式
    var timeStamp = + new Date()
    // 显式
    new Date().getTime()
    Date.new() // es5

3,符号"~"
------
`~`和indexOf一起，可以将结果强制类型转换为真/假值，它是字位操作。        
`if(~a.indexOf(...)){}`

`~`还可以作为字位截除

4,parseInt
------
parseInt针对的是字符串值，如果第一个参数不是字符串，则会默认强制转换为字符串。

从ES5开始，parseInt(..)默认转换为十进制数。

`parseInt(1/0, 19)`结果为`18`  ，相当于`parseInt('Infinity', 19)`.      
第一个字符是"I",以19为基数时值为18，    
第二个字符"n"不是一个有效的数字字符，解析到此为止。


    parseInt(0.000008)    // 0    (“0”来自"0.000008")
    parseInt(0.0000008)  // 8    ("8来自“8e-7”")
    parseInt(false, 16)     // 250  ("fa"来自"false")
    parseInt(parseInt, 16)  // 15  ("f"来自"function..")
    parseInt("0x10")          // 16   ("0x"，第二位没有设置，即使是ES5以后，也是按16进制换算的)
    parseInt("0x10"， 10)   // 0
    parseInt("103", 2)        //2  ("10"二进制为“2”，“3”不是一个有效数字字符)


5,符号`!`,`+`, `-`
------
`!!a`可以更简要的将类型转换为Boolean

数组相加，因为数组的valueOf()操作无法得到简单基本类型，于是它转而调用toString()

    var a = [1,2]
    var b = [3,4]
    a+b    // '1,23,4'

验证：设置toString

    var a = [1,2]
    var b = [3,4]
    a.toString = () => 3
    b.toString = () => 7
    a + b = 10


验证：设置valueOf

    var a = [1,2]
    var b = [3,4]
    a.toString = () => 3
    b.toString = () => 7
    a.valueOf = () => 4
    a + b = 11


对象和数组相加

    [] + {}   // [object object]
    {} + []  // 0

`-`法

    var a = [3]
    var b = [1]
    a -b  // 2   ("a"和"b"首先被转化为字符串，然后再转化为数字)


6,Boolean值到数组的隐式强制类型转换
------
需求：判断一系列数据列表中，有且只有一个为true。

     function onlone () {
           var sum = 0
          for (var i=0;i < arguments.length; i++) {
               if (arguments[i]) {
                    sum += !!arguments[i]
               }
          }
          return sum ==1
     }

7,&& 和 || 
------
&& 和 || 他们的返回值是两个操作符中的一个

换一个角度来理解：

    a || b
    // 大致相当于
    a? a : b

    a && b
    // 大致相当于
    a ? b : a

之所以说是大致相当，是因为：若a为一个表达式，且返回值为true，
则 `a ? a:b` 中会执行两次，而 `a || b`中只会执行一次

8,null 和undefined
----
在`==`中numm和undefined是一回事，可以相互进行隐式强制类型转换

    var a = null
    var b

    a == b              // true
    b == null           // true
    a == undefined      // true

    a == false          // false
    b == false          // false
    a == ""             // false
    b == ""             // false
    a == 0              // false
    b == 0              // false

9,对象和非对象之间的相等比较
----
Type(x)是字符串或数字,Type(y)是对象 ，则返回x == ToPrimitive(y)的结果。反之亦然。

    var a = 42
    var b = [42]

    a == b // true


ToPrimitive抽象操作，先调用valueOf()，若操作无法得到简单基本类型，则转而调用toString()。        
eg:

    Number.prototype.valueOf = function () {
        return 3
    }
    new Number( 2 ) == 3

思考:
如何让 `if (a ==2 && a ==3 ) {...}`成立

    var i = 2
    Number.prototype.valueOf = function () {
        return i++
    }
    var a = new Number( 42 )
    if (a ==2 && a ==3) {
        console.log("Yep, this happened.")
    }

极端情况：      

    [] == ![] // true
    上面等价于 [] == false

