---
title: Promise 简述
date: 2021/4/21
updated: 2021/4/21
comments:
tags:
 - es6
 - js
categories:
 - learningNotes
layout:
permalink:
meta: true
---


## 1. Promise简介

+ Promise 是异步编程的一种解决方案

## 2. 网络请求的回调地狱

> 简述：
>
> 需要通过url1从服务器加载一个数据data1，data1中包含了下一个请求的url2；
>
> 需要通过data1取出url2，从服务器加载数据data2，data2包含了下一个请求的url3；
>
> 需要通过data2取出url3，从服务器加载数据data3，data3包含了下一个请求的url4；
>
> 发送网络请求url4，获取最终的数据data4

~~~js
$.ajax('url1',function(data1){
    $.ajax(data1['url2'],function(data2){
        $.ajax(data1['url3'],function(data3){
        	$.ajax(data1['url4'],function(data4){
        		console.log(data4);
    		})
    	})
    })
})
~~~

> 这样的代码难看且不易维护
>
> 更加期望用一种更加优雅的方式来进行这种异步操作——promise



## 3.定时器的异步事件

> 使用定时器模拟异步操作
>
> ~~~js
> // 1. 使用setTimeout
> setTimeout(function(){
>     let data = 'Hello World'
>     console.log(data);
> },1000)
> ~~~

上述是我们过去的处理方式，将其转换成promise代码：

~~~js
// 参数 -> 函数(resolve,reject)
// resolve,reject本身也是函数
// 链式编程
new Promise((resolve,reject) => {
    setTimeout(function(){
        //成功时调用 resolve
        resolve('Hello World')
        //失败时 调用reject
        reject('Error Data')
    },1000)
}).then(data => {
    console.log(data);
}).catch(error => {
    console.log(error);
})
~~~

+ 什么时候会用到promise？

  一般是有异步操作时，使用promise对这个异步操作进行封装

+ 怎么使用

  new -> 构造函数（1.保存了一些状态信息 2.执行传入的函数）

  在执行传入的回调函数时，会传入两个参数，resolve，reject，其本身也是函数



## 3.Promise三种状态

+ 在开发中有异步操作时，就可以给异步操作包装一个Promise。异步操作之后会有三种状态:
  + pending：等待状态，如：正在进行网路请求，或者定时器没有到时间
  + fulfill：满足状态，当主动回调了resolve时，就处于该状态，并且会回调.then()
  + reject：拒绝状态，当主动回调了reject时，就处于该状态，并且会回调.catch()

~~~js
// promise 的另外处理形式
new Promise((resolve,reject) =>{
    setTimeout(() => {
        resolve('Hello Vuejs')
        // reject('error message')
    },1000)
}).then(data => {
    console.log(data);
},err => {
    console.log(err)
})
~~~



## 4 Promise 链式调用

+ 在Promise的流程图中，无论时then还是catch都可以返回一个Promise对象
+ 所以，代码可以进行链式调用：
  + 直接通过Promise包装新的数据，将Promise对象返回
  + Promise.resolve()：将数据包装成Promise对象，并且在内部调用回调resolve()函数
  + Promise。reject()：将数据包装成Promise对象，并且在内部回调reject()函数

~~~js
//wapped into 
//网络请求：aaa -> 自己处理
// 处理: aaa111 -> 自己处理
// 处理: aaa111222 -> 自己处理

new Promise((resolve,reject) => {
    setTimeout(() => {
        resolve('aaa')
    },1000)
}).then(res => {
    //自己处理
    console.log(res)
    
    //对结果进行第一次处理
    return new Promise((resolve) => {
        resolve(res + '111')
    })
}).then(res => {
     //自己处理
    console.log(res)
    
    //对结果进行第二次处理
    return new Promise((resolve) => {
        resolve(res + '222')
    })
}).then(res => {
    console.log(res)
}).catch(err => {
    console.log(err)
})
~~~



链式调用简写

+ if希望数据直接包装成Promise.resolve，那么在then中可以直接返回数据
+ 当把return Promise.resolve(data) 改成return data时结果也是一样的

~~~js
    //对结果进行第一次处理
    return new Promise((resolve) => {
        resolve(res + '111')
    })

	// 1. 使用return Promise.resolve/reject
	return Promise.resolve(res + '111')
	/
    return Promise.reject('error message')  

	// 2. 省略Promise.resolve
	return res + '111'
	/ 
    throw 'error message'
~~~



## 5. 链式调用二

> 简述：如果一个结果需要两个请求成功后才能得到
>
> 那没有使用Promise时是这样的

~~~js
let isResult1 = false;
let isResult2 = false;

//请求1
$.ajax({
    url:'',
    success:function(){
        console.log('result1');
        isResult1 = true;
        handleResult()
    }
})

//请求2
$.ajax({
    url:'',
    success:function(){
        console.log('result2');
        isResult2 = true;
        handleResult()
    }
})

function handleResult(){
    if(isResult1 && isResult2){
        
    }
}
~~~

**Promise.all**

使用Promise

~~~js
Promise.all([
    new Promise((reslove,reject) => {
        $.ajax({
            url:'',
            success:function(data){
                resolve(data)
            }
        })
    }),
    new Promise((resolve,reject) => {
        $.ajax({
            url:'',
            success:function(data){
                resolve(data)
            }
        })
    })
]).then(results => {
    results[0]
    results[1]
})
~~~















