---
layout: post    
title: 跨域请求集中处理        
categories: Blog        
description: 主要介绍一种个人用过并且觉得非常好用的api请求的统一管理办法。     
keywords:  axios, api        
---

统一管理api请求，定义请求函数可以简化项目代码， 
    
封装前
    
    axios({
        url: '/api/cmn/user/info',
        type: 'get',
        header: {accessToken : 'token XXXXXXXXXXXXXXX'},
        params: {
            id:12
        }
    }).then(res => {
        console.log(res)
    }).catch(err => {
        console.log(err)
    )
    
封装后

    cmnUserInfo({id: 12}).then(res => {
        console.log(res)
    })

当你的请求从原来的12行变成3行时，会不会觉得特别轻松简洁？      

当一个页面中有五个请求以上时，60行以上的请求代码在你面前晃来晃去，有没有觉得眼晕？      
 
当不同的页面需要请求相同的接口时，重复性的书写请求有没有觉得很累？很没有必要？

当接口路由地址被修改替换时，需要一个个手动地去查找替换，改完是否还担心是否有所遗漏？

统一封装管理api请求，将会解决这些问题！


跨域请求集中定义
----
对照前后端对接的api文档，将所有的跨域请求以函数的形式定义在同一个js文件内。     
在需要调用接口时，直接引入对应的函数。     

axios/index.js中

    import axios from 'axios'
    import Cookies from "js-cookie"
    import {router} from "../router"
    import qs from "qs"
    
     let instance = axios.create({
          //自定义的请求头
          headers:{
            post:{
               "Content-Type":"application/x-www-form-urlencoded;charset=UTF-8"
            },
            "X-Requested-With":"XMLHttpRequest",
            "Content-Type": "application/json;charset=UTF-8",
          },
         // 超时设置
          timeout:10000, 
          //跨域是否带Token
          widthCredentials:true,
          //响应的数据格式  json / blob / document / arraybuffer /text / stream
          responseType:"json",
          //用于node.js
          httpAgent:new http.Agent({
            keepAlive:true
          }),
          httpsAgent:new https.Agent({
            keepAlive:true
          }),
    })
    
     //添加请求拦截器
    instance.interceptors.request.use(
        config => {
            let token = Cookies.get("jubao_user");
            if (token) {
               config.headers.accessToken = token; 
            } else {
              //重定向到登陆页面
              router.push("/login")
            }
            
            //3,根据请求方法，序列化传来的参数，根据后端需求是否序列化
            if (config.method === "post") {
                if (config.data.__proto__ === FormData.prototype
                    || config.url.endsWith("path")
                    || config.url.endsWith("mark")
                    || config.url.endsWith("patchs")
                ) {
                } else {
                config.data = qs.stringify(config.data)
                }
            }
            return config
        },
        error => {
            //请求错误时
            console.log("request:", error);
            //1,判断请求超时
            if (error.code === "ECONNABORTED" && error.message.indexOf("timeout") !== -1) {
              console.log("timeout请求超时")
              //return service.request(originalRequest); //再重复请求一次
            }
            //需要重定向到错误页面
            const errorInfo = error.response;
            if (errorInfo) {
              error = errorInfo.data  //页面那边catch的时候就能拿到相信的错误信息
              const errorStatus = errorInfo.status; //404 403 500
              router.push({
                path: `/error/${errorStatus}`
              })
            }
            return Promise.reject(error)  //再调用的那边可以拿到（catch）你想返回的错误信息
        }
    )
    
    // 添加响应拦截器
    axios.interceptors.response.use( response => {
        // 对响应数据做点什么
        return response.data;   // 比如原公司的信息永远都放在data里面，所以这里可以默认只取data
    }, function (error) {
        // 对响应错误做点什么
        return Promise.reject(error);
    });

api.js中

    import {$axios} from '../axios' 

    //默认为get请求，
    export const cmnUserInfo = $axios('/api/cmn/user/info', params)
    export const cmnOrderSave = $axios('/api/cmn/order/save', params, 'post')
    
在需要调用跨域请求时

    import {cmnUserInfo} from 'api.js
    
    cmnUserInfo({id: 13}).then(res=>{
        console.log(res)    //res为返还值
    })

    