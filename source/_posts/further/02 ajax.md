---
title: ajax
date: 2021/11/11
updated: 2021/11/11
comments:
tags:
 - js
categories:
 - furtherNotes
layout:
permalink:
meta: true
---


## ajax

### 1.简介

ajax(asynchronous javascript and xml) 异步 js 与 xml

一种创建交互式网页应用的网页开发技术

使用异步方式与服务器通信，不需要打断用户操作

在不刷新整个网页情况下，与服务器通信交互



### 2. 特点

+ 优点：可以不刷新页面与服务端进行通信

  ​           允许根据用户事件来更新部分页面内容

+ 缺点：没有浏览记录，不能回退

  ​            存在同源跨域问题

  ​             seo不友好



### 3. 原生ajax使用

1. 创建对象

2. 初始化 设置请求方法和url

3. 发送 设置参数

   > get：请求数据拼在 url 中
   >
   > post：设置请求头 格式内容 + 请求数据放在 send 里
   >
   > ​           xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded"); 

4. 事件绑定 处理服务端返回结果

   

~~~js
const xml = new XMLHttpRequest();
xhr.responseType = 'json';   // open前 设置响应体数据
xhr.timeout = 2000;
xhr.ontimeout = function(){...}
xhr.onerror = function(){...}
xhr.open('GET','http://....');
xhr.send();
xhr.onreadystatechange = function(){
    if(xhr.readyState == 4 && xhr.status == 200){
        xhr.status; //状态码
        xhr.statusText; //状态码字符串
        xhr.getAllResponseHeaders();  //所有响应头
        xhr.response; //响应体
    }
}
~~~

> xhr.abort()



### 4. readyState 

表示请求发送的状态 0 1 2 3 4

0：未初始化 — 初始状态

1：启动 — open后

2：发送 — send后

3：接收 — 返回了部分数据

4：完成 — 返回了所有数据















