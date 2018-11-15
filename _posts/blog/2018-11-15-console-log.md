---
layout: post
title: 异步的console
categories: console
description: 一个关于console的小问题
keywords: console
---

从一个常见的关于console的小问题，引发的一个已经习以为常被忽略的问题。

1，console.log的简单案例
------
首先看一个简单的例子

    var a = {
        index:1
    };
    console.log(a)
    a.index++

现在看看浏览器的控制台的输出结果为
![](/images/blog/console.png)
结果为`{index:2}`

过去，我对这个结果一直是这么认为的：

    在console.log(a)运行的时候，其实控制台输出的是{index:1}，
    但是由于后来a.index++运行了，原来的a变成a{index:2}了，
    从而将控制台输出的{index:1}改变成了{index:2},所以我们在控制台看到的是{index:2}

接下来利用chrome的打断点，在执行到`console.log(a)`时，控制台确实输出了{index:1},看起来可以这么解释。

**接下来把chrome浏览器刷新一下。**
![](/images/blog/console2.png)
结果貌似变成了`{index:1}`

**点击输出的`{index:1}`对象，将其展开**
![](/images/blog/console3.png)
最终的结果依旧是`{index:2}`

**但是那一行`{index:1}`代表什么含义？**
`Value below was evaluated just now.` --- chrome评估当时的输出应该为`{index:1}`,为啥展开后展示的却是`{index:2}`

百度搜索了一下，找到一个解释：

    因为当你在Chrome Console点击展开数组时，会重新去读一遍内存真实的值然后显示，一但展开后就不会再变.

附上一个证明例子

    var Point = function() {}
    var arr = [ new Point(), new Point(), new Point() ]

    console.log(arr.length)
    console.log('第一次console', arr)
    setTimeout(function() {
        arr.push(new Point())
    }, 1000 * 5)

第一次：5秒内展开数组，看到三个元素
（刷新页面后）
第二次：5秒后展开数组，看到四个元素

原因清楚了，但是当对象结构复杂时，**怎么才能看到当时的值，而不是内存中最后的值？**

    var Point = function() {}
    var arr = [ new Point(), new Point(), new Point() ]

    console.log(arr.length)
    console.log('第一次console', JSON.parse(JSON.stringify(arr)))
    setTimeout(function() {
        arr.push(new Point())
    }, 1000 * 5)

此时，无论任何时候展开数组，看到的都是三个元素

JSON.parse也可以省略，直接输出字符串：

    console.log('数组arr', JSON.stringify(arr, null, 2))

当然，打断点的方法也可以看到即时内容。

原文：
[chrome 调试console点开时数据不对value below was evaluated just now](https://www.baidu.com/link?url=7yEI-r1hjgfYqrRKJt10Tk2S11qlZ5I1mtSEtjEeyUjFf5jTRxH2y_uOtlHACJ3BUE30GezDs-R9YhVi1UR9oaZVKA6trQFf_HMqVQNCmS3&wd=&eqid=b5744d3b000258aa000000035bed5c70)

2，重新认识console.*方法
------
上面的解释似乎已经能够完美解释所有的问题，但是实际情况更加复杂。

    并没有什么规范或一组需求指定console.*方法族如何工作
        ----他们并不是javascript正式的一部分，而是宿主环境添加到javascript中的。
    因此，不同的浏览器和javascript环境可以按照自己的意愿来实现，有时候这会引起混淆。
    尤其要提出的是，在某些条件下，某些浏览器的console.log(..)并不会把传入的内容立即输出。
                                            ----《你所不知道的javascript》中卷P140-141

*这段代码运行时，浏览器可能会认为需要把控制台I/O延迟到后台*
总之，console.*的输出也可能是异步的，并不一定是同步输出。