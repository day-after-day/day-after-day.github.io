---
layout: post
title: vue 组件部分笔记
categories: vue
description: 记下一些值得思考的部分
keywords: Vue, component
---


1,组件的命名
----
建议:     
在定义组件时使用大驼峰命名法(camelCased )        
在使用组件时使用短横线隔开式(kebab-case)。

从原理上来讲，一个vue的组件，其实可以认为是一个类（为什么？例子在哪？），类的命名用大写字母开头来命名。


2,data的定义
----
在使用data定义组件内的数据时：
如果我们这个组件不是通过new Vue这种方式生成的，而是要注册到全局或者某个实例中使用的话，
那么data一定要是一个 function并且return想要的数据结构。

原因：为了避免组件多次使用时，data相互影响。
并且return的必须是一个新对象，不能使用一个全局的对象。


    import Vue from 'vue'
    
    let data = {
      text: 0
    }
    
    const component = {
      template: `
            <div>
                <input type="text" v-model.number="text">
            </div>
        `,
      data () {
        return data
      }
    }
    
    // Vue.component('CompOne', component)
    
    new Vue({
      el: '#root',
      components: {
        CompOne: component
      },
      template: `
        <div>
            <comp-one></comp-one>
             <comp-one></comp-one>
        </div>
      `
    })
    
    
3,props
----
props为父组件向子组件传值时使用，
建议:让props作为单向数据流。

props的设置方式      

方式一

    props:['active','propOne']
    
方式二
    
    props: {
        active: Boolean,
        propOne: String
    }
    
方式三
    
    props: {
        active: {
            type: Boolean,
            require: true,
        }，
        //  如果是要给对象设置default，则必须用函数形式。
        //  避免组件重复使用时，相互影响
        propTwo: {
            type: Object,
            default () {
                return {
                    message: 1
                }
            }     
        },
        //更为严格 自由地验证参数
        propsThree: {
            validator (value) {
                return typeof value === 'boolean'
            }
        }
    }
    
