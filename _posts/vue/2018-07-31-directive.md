---
layout: post
title: vue directive部分笔记
categories: vue
description: 记下一些值得思考的部分
keywords: Vue, directive
---

1，对于v-for中为什么要加key
----

    <ul>
        <li v-for="(item,index) in arr" :key="item">{{ item }}:{{index}}</li>
    </ul>
     <ul>
        <li v-for="(val,key,index) in obj">{{ key }}:{{val}}:{{index}}</li>
    </ul>

以key作为唯一标识，
因为数据是经常会变得，当数据改变时，vue会重新渲染列表并且放在dom中去，这样性能开销就会很大。       

当指定key后，在下一次循环时，如果在缓存中拿到了同样的key，vue就会将这一行dom节点直接复用，不需要生成新的dom节点。
使用缓存过的dom节点，性能开销减小。 

一般不推荐使用index作为key，因为数组的变化跟实际数组的内容其实是没有任何关系的。
*key的意义更倾向于是对数组内容的唯一标识*
使用index作为key是一个不好的选择，这样有可能导致一个错误的缓存。


2，v-on
----

`@click /v-on:click` 其实是在vue对象的实例上去绑定这个事件。`类似于vm.$on`。

v-on首先会判断指令是绑定在dom节点上，还是vue组件上：     
如果v-on是绑定在dom节点上，则会使用document.addEventListener去绑定事件；  
如果v-on是绑定在vue组件上，则会通过组件的vm.$on（vm代表这个组件的实例）去添加一个事件。     

3，v-model
----
事件修饰符`.number` ---- number化model值

    <input type="text" v-model="text">

 data中的text
 
    data(){
        return {
            text: 0
        }
    }

页面渲染时，input框上默认绑定的为数字`0`；     
当输入数字1后，input框上绑定的就是字符串`"01"`，

当加入修饰符`.number`之后，text框在输入其他字符后，绑定的就一直是数字.
如果输入其他字符(abc空格等)，在失焦时则会被默认清除。

事件修饰符`.trim` ---- 去除model值首尾的空格。
若没有.trim的`input[type='text']`框，会允许有首尾空格。


事件修饰符`.lazy` 
在`<input type='text' v-model.lazy='text'>`上输入字符的时候，model值不会改变。只有当失焦时，model值才会变化。


4，v-pre
----

    <div v-pre>Text: {{text}}</div>
    
带有v-pre的标签，标签内的内容都不会被vue解析。 

5，v-cloak
----
这个指令直接使用webpack开发的项目都用不到        
使用的唯一场景为：       
* 在页面上引入了vue的库代码，在页面上写vue代码，模板是写在html,body里面的。
浏览器在解析时，会直接原样渲染，随后才是vue重新渲染。        

v-cloak作用是：
在代码还没有加载完成之前，给带有v-cloak的标签加上样式`display:none`；
在代码加载完成后，vue做的第一件事就是把`display:none`去掉。

6，v-once
----
数据绑定的内容只执行一次，后续的数据变化不再触发模板的重新编译。

使用场景为：      
* 显示静态内容时使用。

这样vue就不会再去检测v-once内的内容了，不去做虚拟dom的对比，判断是否要重新渲染更新，节省性能开销。

