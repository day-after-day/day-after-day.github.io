---
layout: post
title: 浏览器的重绘和重排
categories: html
description: 浏览器的重绘和重排
keywords: html, reflow, repaint
---

简单总结一下浏览器的重绘和重排

（1）Reflow（回流/重排）：
----
浏览器要花时间去渲染，当它发现了某个部分发生了变化影响了布局，那就需要倒回去重新渲染。

（2）Repaint（重绘）：
-----
如果只是改变了某个元素的背景颜色，文字颜色等，不影响元素周围或内部布局的属性，将只会引起浏览器的repaint，重画某一部分。


**Reflow要比Repaint更花费时间，也就更影响性能。所以在写代码的时候，要尽量避免过多的Reflow。**

（3）reflow的原因：
----
（1）页面初始化的时候；
（2）操作DOM时；
（3）某些元素的尺寸变了；
（4）如果 CSS 的属性发生变化了。

（4）减少 reflow/repaint
------
　（1）不要一条一条地修改 DOM 的样式。与其这样，还不如预先定义好 css 的 class，然后修改 DOM 的 className。
　（2）不要把 DOM 结点的属性值放在一个循环里当成循环里的变量。
　（3）为动画的 HTML 元件使用 fixed 或 absoult 的 position，那么修改他们的 CSS 是不会 reflow 的。
　（4）千万不要使用 table 布局。因为可能很小的一个小改动会造成整个 table 的重新布局。


（5）浏览器对此做了哪些处理来优化渲染效率
-----
浏览器尽可能将 repaint/reflow 限制在被改变元素的区域内。
* 比如：对于位置固定或绝对的元素，其大小改变只影响元素本身及其子元素
*然而，静态定位元素的大小改变会触发后续所有元素的重流。*


另一种优化技巧是:
在运行几段JavaScript代码时，浏览器会缓存这些改变，在代码运行完毕后再将这些改变经一次通过加以应用。


下面是一个开发中会遇到的问题，如果你不清楚原因，只会觉得莫名其妙。

（6）有时候希望强制触发回流，否则我们预期的效果就无法达到
-----


    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <style>
            .has-transition {
                -webkit-transition: margin-left 1s ease-out;
                -moz-transition: margin-left 1s ease-out;
                -o-transition: margin-left 1s ease-out;
                transition: margin-left 1s ease-out;
            }
            #targetElemId{
                width:200px;height:200px;
                background: yellowgreen;
            }
        </style>
    </head>
    <body>
        <div id="targetElemId">

        </div>
    </body>
    <script src="../../jQuerySource/jquery-3.3.1.min.js"></script>
    <script>
        // our element that has a "has-transition" class by default
        var $targetElem = $('#targetElemId');

        // remove the transition class
        $targetElem.removeClass('has-transition');

        // change the property expecting the transition to be off, as the class is not there
        // anymore
        $targetElem.css('margin-left', 100);

        // put the transition class back
        $targetElem.addClass('has-transition');

        // change the property
        $targetElem.css('margin-left', 50);
    </script>
    </html>


原代码期望的动画效果是：div块从右往左移动，但是由于浏览器的回流缓存优化机制，元素一开始就一直在`margin-left:50px`的位置，没有动画。

我们需要在改变margin-left为100后，触发浏览器的回流，再改变margin-left为50，这样才会出现动画效果。

如下：

        // our element that has a "has-transition" class by default
        var $targetElem = $('#targetElemId');

        // remove the transition class
        $targetElem.removeClass('has-transition');

        // change the property expecting the transition to be off, as the class is not there
        // anymore
        $targetElem.css('margin-left', 100);

        $targetElem[0].offsetHeight // 访问/改变元素的属性，会触发强制性的重排

        // put the transition class back
        $targetElem.addClass('has-transition');

        // change the property
        $targetElem.css('margin-left', 50);


（7）相关参考文章
-----
[https://blog.csdn.net/osdfhv/article/details/52159341](https://blog.csdn.net/osdfhv/article/details/52159341)

[https://www.cnblogs.com/peteremperor/p/6285449.html](https://www.cnblogs.com/peteremperor/p/6285449.html)
