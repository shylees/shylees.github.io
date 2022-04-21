---
title: Promise基本用法
date: 2021/11/26
updated: 2021/12/3
comments:
tags:
 - promise
 - es6
categories:
 - furtherNotes
layout:
permalink:
meta: true
---

> 2021.11.26 建的文件 12.3 终于打算写了
>
> 回调地狱 解决方案中 Promise
>
> 常见 promise 输出题：https://juejin.cn/post/6844904077537574919
>
> https://juejin.cn/post/6844903607968481287
>
> https://juejin.cn/post/6844903607968481287
>
> https://juejin.cn/post/6952083081519955998


## Promise

### 1. 简介

Promise 是**异步编程的一种解决方案**：

从语法上讲，promise 是一个对象，它可以获取异步操作的消息；

从本意上讲，promise 是承诺，承诺它过一段时间会给你一个结果。

promise 有三种状态：**pending**(等待态)、**fulfiled**(成功态)、**rejected**(失败态)；状态一旦改变，就不会再变。创建 promise 实例后，其会立即执行。

>  Pending 变为 Fulfilled 会得到一个私有**value**，Pending 变为 Rejected会得到一个私有**reason**，当Promise达到了Fulfilled或Rejected时，执行的异步代码会接收到这个value或reason

promise 解决的问题：

+ 回调地狱，代码难以维护，常常第一个函数的输出是第二个函数的输入的这种现象
+ promise 可以支持多个并发的请求，获取并发请求中的数据
+ promise 可以解决异步的问题，但是不能说 promise 是异步的



### 2. 基本用法

+ resolve ：异步操作执行成功后的回调函数

+ reject ：异步操作执行失败后的回调函数

+ then ：捕获 promise 状态变化（成功 / 错误），并将拿到的数据进行操作

+ catch：相当于 then(null,error => { … } )，then 的第二个参数

+ all：接收一个promise 实例数组参数，返回一个以传入数组顺序的返回结果的数组，提供了并行执行异步操作的能力，在所有异步操作执行完后才执行回调

  > ~~~js
  > let Promise1 = new Promise(function(resolve, reject){})
  > let Promise2 = new Promise(function(resolve, reject){})
  > let Promise3 = new Promise(function(resolve, reject){})
  > 
  > let p = Promise.all([Promise1, Promise2, Promise3])
  > 
  > p.then(funciton(){
  >   // 三个都成功则成功  
  > }, function(){
  >   // 只要有失败，则失败 
  > })
  > ~~~

+ race：接收一个Promise 实例数组参数，返回最快执行完的操作结果

  > 使用场景：可以用 race 给某个异步请求设置超时时间，并且在超时后执行相应的操作
