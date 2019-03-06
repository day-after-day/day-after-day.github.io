---
layout: post
title: 将存在父子级关系的数据转化为树状结构
categories: data
description: 将存在父子级关系的数据转化为树状结构
keywords: data
---

由于产品要求按照树状结构展示，于是自己将element-ui的tree组件修改了一下，二次封装成了一个[简单的树状表格组件](http://gaofangshang.com//2018/08/01/reset-axios/)，需要的数据必须是树状结构。

后端不肯转换数据，于是只好自己转换,顺手封装。


源数据：

    let arr = [
      {deviceId: '1',parentId: ''},
      {deviceId: '2',parentId: ''},
      {deviceId: '1-1',parentId: '1'},
      {deviceId: '1-1-1',parentId: '1-1'},
      {deviceId: '1-1-2',parentId: '1-1'},
    ]

目标数据结构：

    [
      {
        "deviceId": "1",
        "parentId": "",
        "children": [
          {
            "deviceId": "1-1",
            "parentId": "1",
            "children": [
              {
                "deviceId": "1-1-1",
                "parentId": "1-1"
              },
              {
                "deviceId": "1-1-2",
                "parentId": "1-1"
              }
            ]
          }
        ]
      },
      {
        "deviceId": "2",
        "parentId": ""
      }
    ]

下面贴上转换函数：

dataToTree.js中

    import {deepCopy} from "./copy";

    /**
     * Mr.Gao
     * @param data  源数组
     * @param id  唯一键名
     * @param parentId  父级id键名
     * @param children  子集键名
     * @param copy  是否开启copy模式。 默认为false，会修改原来的数组。 为true时，会进行深拷贝再转换，不影响原来的数组。
     *                                  区别在于性能差异， 如果源数组没有在使用，则建议设置为false
     * @returns {Array}   转换后的属性结构
     */
    export const  dataToTree = (data, {id='id', parentId='parentId', children='children'}, copy= false) =>{
      copy && ( data = deepCopy(data) )
      let tree = [];
      data.forEach(item=>{
        function run(arr) {
          return arr.some(val=>{
            if(val[id] === item[parentId]){
              val[children] ?  val[children].push(item) : val[children] = [item];
              return true
            }else{
              return val[children] && run(val[children])
            }
          })
        }

        if(!run(tree)){
          tree.push(item)
        }
      })
      return tree
    }


copy.js中

    /**
     * 深拷贝对象或者数组
     * @param obj   对象或数组
     * @returns {*} 返回一个同样的对象/数组，但是与原对象/数组没有关联
     */
    export const deepCopy = (obj) => {
        let objClone = Array.isArray(obj) ? [] : {}
        if (obj && typeof obj === 'object') {
            for (let key in obj) {
                if (obj.hasOwnProperty(key)) {
                    // 判断ojb子元素是否为对象，如果是，递归复制
                    if (obj[key] && typeof obj[key] === 'object') {
                        objClone[key] = deepCopy(obj[key])
                    } else {
                        // 如果不是，简单复制
                        objClone[key] = obj[key]
                    }
                }
            }
        }
        return objClone
    }


**以上就是最终封装好的代码**。

其实，在一开始转换的时候，为了稳妥起见，思路没那么清晰，也没有那么骚的去使用那么多关键字。

原始的代码也贴一下:

    function dataToTreeOrigin(data){
      let tree = [];
      data.forEach(item=>{
        let flag = false
        find(tree)
        function find(arr) {
          for(let i=0;i<arr.length;i++){
            let val = arr[i]
            if(val.deviceId === item.parentId){
              val.children ?  val.children.push(item) : val.children = [item];
              flag = true
              break;
            }else{
              val.children && find(val.children)
            }
          }
        }

        if(!flag){
          tree.push(item)
        }
      })
      return tree
    }


很多代码一开始并没有那么骚气。功力不足的话,都是从很土的代码开始尝试,功能完成后再开始简化封装，自然就变得高级了。

等简化的次数多了，自然功力就增长了，代码的整理和封装也算是个人成长的一大养料。๑乛◡乛๑


