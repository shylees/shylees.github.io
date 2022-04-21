---
title: 10 axios
date: 2021/5/2
updated: 2021/5/3
comments:
tags:
 - vue
categories:
 - learningNotes
layout:
permalink:
meta: true
---

[toc]

## 1.axios功能特点

+ 在浏览器中发送XMLHttpRequests请求
+ 在node.js中发送http请求
+ 支持Promise API
+ 拦截请求和响应
+ 转换请求和响应数据

## 2.axios请求方式

+ `axios(config)` 默认get请求
+ `axios.request(config)`
+ `axios.get(url[,config])`
+ `axios.delete(url[,config])`
+ `axios.head(url[,config])`
+ `axios.post(url[,data[,config]])`
+ `axios.put(url[,data[,config]])`
+ `axios.patch(url[,data[,config]])`



## 3.axios框架的基本使用

接口：http://123.207.32.32:8000/home/multidata

axios默认是get方法

~~~js
//get请求
axios({
    url:"http://123.207.32.32:8000/home/multidata"
}).then(res => {
    console.log(res)
})

//带参数
axios({
    url:"http://123.207.32.32:8000/home/data?type=pop&page=1"
}).then(res => {
    console.log(res)
})

//get请求的参数拼接
axios({
    url:"http://123.207.32.32:8000/home/data",
    params:{
        type:'pop',
        page:1
    }
}).then(res => {
    console.log(res)
})
~~~



## 2.axios发送并发请求

~~~js
axios.all([axios({
        url: "http://123.207.32.32:8000/home/multidata"
    }), axios({
        url: "http://123.207.32.32:8000/home/data",
        params: {
            type: 'pop',
            page: 2
        }
    })])
    .then(results => {
         console.log(results);
    });

// 使用axios.spread可将数组[res1,res2]展开
axios.all([axios({
        url: "http://123.207.32.32:8000/home/multidata"
    }), axios({
        url: "http://123.207.32.32:8000/home/data",
        params: {
            type: 'pop',
            page: 2
        }
    })])
    .then(axios.spread((res1, res2) => {
        console.log(res1);
        console.log(res2);
    }))
~~~



## 3.全局配置

因为BaseURL是固定的，可以抽取/利用axios的全局配置

~~~js
// 3.全局配置 
//超时 ms
axios.defaults.baseURL = "http://123.207.32.32:8000"
axios.defaults.timeout = 5

//get请求
axios({
    url: "/home/multidata"
}).then(res => {
    // console.log(res);
})
~~~



### 3.2常见的配置选项

如果是put  params

如果是post data

<img src='https://s1.328888.xyz/2022/04/09/XaK8e.jpg'>



## 4.axios实例和模块封装

### 4.1如果有多个请求接口时 使用实例会比较好

~~~js
//main.js
// 4.创建对应的实例
const instance1 = axios.create({
    baseURL = "htp://123.207.32.32:8000",
    timeout: 5000
})

instance1({
    url: "/home/multidata"
}).then(res => {
    console.log(res);
})

instance1({
    url: "/home/data",
    params: {
        type: 'pop',
        page: 1
    }
}).then(res => {
    console.log(res);
})


const instance2 = axios.create({
  baseURL = "htp://123.207.33.11:8000",
  timeout: 5000
})

~~~



### 4.2 模块封装

如果有个第三方的东西需要导入，不要每一个组件都导入这个第三方框架，这样每个页面对这个框架依赖性太强了，如果有一天这个第三方的东西不再维护了，所有页面都得换掉

所以尽量封装成一个文件，这样改的话就改一个文件的就可以了

#### 4.2.1.每个页面都导入的情况

~~~js
//vue
<h2>{{ categories }}</h2>
import axios from 'axios'
export default {
    name:"HelloWorld",
    data(){
        return {
            categories:""
        }
    },
    //生命周期函数  组件创建好后 发送请求
    created(){
        axios({
            url:"http://123.207.32.32:8000/category"
        }).then(res => {
            this.categories = res;
        })
    }
}
~~~



#### 4.2.2 封装

##### 4.2.2.1 src建文件夹 network 建request.js

~~~js
//request.js
import axios from "axios";
export function request(config, success, failure) {
    // 1.创建axios的实例
    const instance = axios.create({
        baseURL: "http://123.207.32.32:8000",
        timeout: 5000
    })

    // 发送真正的网络请求
    // 通过两个函数 吧把结果回调出去
    instance(config).then(res => {
        console.log(res);
        success(res)
    }).catch(err => {
        console.log(err);
        failure(err)
    })
}

//main.js
//5.封装reque模块
import { request } from './network/request'

request({
    url: '/home/multidata'
}, res => {
    console.log(res);
}, err => {
    console.log(err);
})
~~~



##### 4.2.2.2 另一种封装方法：

~~~js
//request.js
import axios from "axios";
export function request(config) {
    // 1.创建axios的实例
    const instance = axios.create({
        baseURL: "http://123.207.32.32:8000",
        timeout: 5000
    })

    instance(config.baseConfig).then(res => {
        config.success(res)
    }).catch(err => {
        config.failure(err)
    })
}

//mian.js
request({
    baseConfig:{
        
    },
    success:function(res){
        
    },
    failure:function(err){
        
    }
})
~~~



##### 4.2.2.3 推荐使用promise

~~~js
//request.js
import axios from "axios";
export function request(config, success, failure) {
    // 1.创建axios的实例
    const instance = axios.create({
        baseURL: "http://123.207.32.32:8000",
        timeout: 5000
    })

    instance(config).then(res => {
        console.log(res);
        success(res)
    }).catch(err => {
        console.log(err);
        failure(err)
    })
}

//main.js
request({
    url:"/home/multidata"
}).then(res => {
    console.log(res);
}).catch(err => {
    console.log(err);
})
~~~



##### 4.2.2.4  最终使用 axios的实例的返回值就是promise

> 因为instance可以直接使用.then .catch方法

~~~js
//request.js
import axios from "axios";
export function request(config) {
        // 1.创建axios的实例
        const instance = axios.create({
            baseURL: "http://123.207.32.32:8000",
            timeout: 5000
        })
        //发送真正的网络请求
       return instance(config)
}

//main.js
request({
    url:"/home/multidata"
}).then(res => {
    console.log(res);
}).catch(err => {
    console.log(err);
})
~~~

> 如果到时候axios不能用了，只需要改4-10行的代码就可以 了



## 5.axios拦截器

四种拦截器：请求成功/失败 、响应成功/失败：服务器没有具体是数据过来，传了错误码

~~~js
export function request(config) {
    // 1.创建axios的实例
    const instance = axios.create({
        baseURL: "http://123.207.32.32:8000",
        timeout: 5000
    })

    // 2.axios的拦截器
    // 全局 axios.interceptors
    // 实例 instance.interceptors
    // 请求拦截
    instance.interceptors.request.use(config => {
        console.log(config); //拦截下来的是配置 拦截了配置还得给返回回去 不然就会请求失败
        // 1.可以通过拦截的形式给config中的一些信息不符合服务器要求 设置header什么的
        // 2.可能在请求时 show 加载中的图标
        // 3.某些网络请求 是必须携带一些信息 例如登录 带token
        return config;
    }, err => {
        console.log(err);
    });
    // 响应拦截
    instance.interceptors.response.use(res => {
        //因为一般用的时候只用data就可以了
        // 拦截了结果得返回出去 不然main.js log.res时undefined
        // console.log(res);
        return res.data;

    }, err => {
        console.log(err);
    });

    // 3.发送真正的网络请求
    return instance(config)
}
~~~



