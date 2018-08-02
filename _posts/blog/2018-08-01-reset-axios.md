---
layout: post    
title: axios跨域请求集中处理        
categories: Blog        
description: 主要介绍一种个人用过并且觉得非常好用的api请求的统一管理办法。     
keywords:  axios, api        
---

统一管理api请求，定义请求函数可以简化项目代码

示例
----
封装可以简化代码
    
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
 

当一个页面中有五个请求以上时，会有60行以上的请求代码；
当不同的页面需要请求相同的接口时，需要重复性的书写相同的请求；
当接口路由地址被修改替换时，需要一个个手动地去查找替换，还可能会有遗漏。

统一封装管理api请求，将会解决这些问题！


跨域请求集中定义
----
对照api文档，将所有的跨域请求以函数的形式定义在同一个js文件内。     
在需要调用接口时，直接引入对应的函数。     

axios/config中

    import http from "http"
    import https from "https"
    
    export default {
          //自定义的请求头
          headers:{
            post:{
              "Content-Type":"application/x-www-form-urlencoded;charset=UTF-8"
            },
            "X-Requested-With":"XMLHttpRequest",
            "Content-Type": "application/json;charset=UTF-8"
          },
          //超时设置
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
    }



axios/index.js中

    import axios from "axios"
    import config from "./config"
    import qs from "qs"
    import Cookies from "js-cookie"
    import {routeBack} from "../router"

    //使用vuex做全局loading使用
    //import store from "@/store"
    export default function $axios(url, params, type='GET') {
      return new Promise((resolve, reject) => {
        const instance = axios.create(config);
    
        //request 拦截器
        instance.interceptors.request.use(
          config => {
            //1,请求开始的时候可以结合vuex开启全屏loading动画
            //console.log(store.state.loading)
            //console.log("准备发送请求")
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
            console.log(errorInfo);
            if (errorInfo) {
              //error = errorInfo.data  //页面那边catch的时候就能拿到相信的错误信息
              const errorStatus = errorInfo.status; //404 403 500
              router.push({
                path: `/error/${errorStatus}`
              })
            }
            return Promise.reject(error)  //再调用的那边可以拿到（catch）你想返回的错误信息
          }
        );
    
        //response 拦截器
        instance.interceptors.response.use(
          response => {
            let data;
            //IE9时response.data是undefined，因此需要使用response.request.res
            if (response.data === undefined) {
              data = JSON.parse(response.request.responseText)
            } else {
              data = response.data;
            }
    
            //根据返回的code值来做不同的处理
            switch (data.rc) {
              case 1 :
                console.log(data.desc);
                break;
              case 0 :
              // store.commit("changeState");
              //console.log("登陆成功")
              default:
            }
            // 若不是正确的返回code，且已经登录，就抛出错误
            // const err = new Error(data.desc)
            // err.data = data
            // err.response = response
            // throw err
    
    
            return data
          },
          err => {
            if (err && err.response) {
              switch (err.response.status) {
                case 400:
                  err.message = '请求错误';
                  break;
                case 401:
                  err.message = '未授权，请登录';
                  break;
                case 403:
                  err.message = '拒绝访问';
                  break;
                case 404:
                  err.message = `请求地址出错: ${err.response.config.url}`;
                  break;
                case 408:
                  err.message = '请求超时';
                  break;
                case 500:
                  err.message = '服务器内部错误';
                  break;
                case 501:
                  err.message = '服务未实现';
                  break;
                case 502:
                  err.message = '网关错误';
                  break;
                case 503:
                  err.message = '服务不可用';
                  break;
                case 504:
                  err.message = '网关超时';
                  break;
                case 505:
                  err.message = 'HTTP版本不受支持';
                  break;
                default:
              }
            }
            console.error(err);
            return Promise.reject(err) // 返回接口返回的错误信息
          }
        );
    
        // 请求处理
        instance({
             url: url,
             headers:{accessToken: Cookies.get("jubao_user")},
             type: type,
             params: type.toUpperCase()==="GET" ? params : '',
             data: type.toUpperCase()==="POST" ? params : '',
         }).then(res => {
          resolve(res);
        }).catch(error => {
          reject(error)
        })
      })
    }
    

api.js中

    import $axios from '../axios' 

    // 默认为get请求，
    // 函数命名规则：以api路由可识别路径（cmd）开始，以驼峰命名进行拼接 --- 含义一目了然
    export const cmnUserInfo( params ) = $axios('/api/cmn/user/info', params)
    export const zcbOrderSave( params ) = $axios('/api/cmn/order/save', params, 'post')
    ....

    
在需要调用跨域请求时

    import {cmnUserInfo} from 'api.js
    
    cmnUserInfo({id: 13}).then(res=>{
        console.log(res)    //res为返还值
    })

    