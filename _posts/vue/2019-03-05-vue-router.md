---
layout: post
title: vue路由配置
categories: vue
description: vue路由配置
keywords:  vue, vue-router
---

vue项目也做了不少了，最近才发现vue-router中除了常用的几个属性外，还有几个没有用过的属性，正好总结一下。

    export default () => {
        return new Router({
            routes,
            mode: 'history', // history模式

            base: '/base/'， // 基础路径，每当使用vue官方router的api跳转时，都会自动在路径前面加上/base/，同时原来的路径也依旧存在。
                                eg： /login    =>     /base/login, 所有到/login的link，最终都会变成到 /base/login

            linkActiveClass: 'active-link',             // 对 与当前路由匹配的 <router-link/>标签class的设定。
                                                        // eg: 在/login下， <router-link to="/login"/>的class名会带有，active-link， exact-active-link

            linkExactActiveClass: 'exact-active-link',  // 精确指定。 eg: 在/login/user 下，  <router-link to="/login"/>标签带有active-link的class名
                                                        //              <router-link to="/login/user"/>标签带有exact-active-link的class名

            scrollBehavior(to, from, savedPosition) {   // 对于每个页面滚动条位置的记录。可以在回到某个路由时，自动处于离开时的滚动条位置
                if(savedPosition){
                    return savedPosition
                }else{
                    return {x:0, y:0}
                }
             },
             parseQuery(query){                 // 对于地址中的query的相关操作，不常用

             },
             stringifyQuery(obj){                  // 对于地址中的query的相关操作，不常用

             }，

             fallback: true,

        })

    }