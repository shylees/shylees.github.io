---
title: HTTP 指南 - 字节青训营
date: 2022/1/22
updated: 2022/1/22
comments:
tags:
 - 网络
 - 青训营
categories:
 - learningNotes
layout:
permalink:
meta: true
---


## 1. 初始：什么是HTTP

### 什么是HTTP

+ Hyper Text Transfer Protocol
+ 应用层协议、基于TCP
+ 请求、响应
+ 简单可扩展
+ 无状态

## 2. 协议分析：报文结构

### 发展

+ HTTP 0.9 — 单行协议

  请求GET/mypage.html、响应只有HTML文档

+ HTTP 1.0 — 构建可扩展性

  增加了Header、有了状态码、支持多种文档、…

+ HTTP 1.1 — 标准化协议

  链接复用、缓存、内容协商、…

+ HTTP 2 — 更加优异的表现

  二进制协议、压缩header、服务器推送、…

+ HTTP 3 — 草案

### 报文

#### + start line ：Method Path Version

+ ##### Method：

  GET — 请求一个指定资源，只用于获取数据

  POST — 用于将实体提交到指定的资源，通常导致在服务器上的状态变化或副作用 

  PUT — 用于请求有效载荷替换目标资源的所有当前表示

  DELETE — 删除指定资源

  HEAD — 请求与get响应相同的响应头

  CONNECT — 建立一个到目标资源标识的服务器的隧道

  OPTIONS — 用于描述目标资源的通信选项

  TRACE — **沿着到目标资源的路径执行一个消息环回测试**

  PATCH — **用于对资源应用部分修改**

  > Safe：不会修改服务器的数据的方法：GET、HEAD、OPTIONS
  >
  > Idempotent幂等：同样的请求被执行一次或多次的效果是一样的，服务器的状态也是一样的：GET、HEAD、OPTIONS、PUT、DELETE

+ ##### 状态码

  200、301、302、401 (请求未经授权)、404、500、504(网关或代理服务器处理超时)

+ #### RESTful API

  一种API设计风格，REST — Representational State Transfer

  + 每一个URI代表一种资源
  + 客户端和服务器之间，传递这种资源的某种表现层
  + 客户端通过HTTP method，对服务器资源进行操作，实现“表现层状态转化”

#### + HTTP Headers 

##### 请求头

Accept：接收类型，表示浏览器支持的MIME类型，对标服务器端返回的Content-type

Content-Type：客户端发送出去的实体内容的类型

Cache-Control：指定请求和响应遵循的缓存机制

If-Modified-Since：对应服务端的**Last-Modified**，用来匹配看文件是否变动，精确1s内

Expires：缓存控制，在这个时间内直接使用请求

Max-age：资源再本地缓存多少秒

If-None-Match：对应服务端**Etag**，匹配文件内容是否改变，较精确

Cookie：有cookie并且同域访问时会自动带上

Referer：该页面的来源URL，适用所有类型请求，详细到页面地址，csrf拦截会用到

Origin：最初请求从哪发起的，只精确到端口号，比上更尊重隐私

User-Agent：用户客户端的一些必要信息 

##### 响应头

Content-Type：服务端返回的实体内容的类型

Cache-Control：指定请求和响应遵循的缓存机制

**Last-Modified**：请求资源的最后修改时间

Expires：在什么时候认为文档已经过期，不再缓存

Max-age：资源再本地缓存多少秒，开启Cache-Control后有效

**Etag**：资源的特定版本标识符

Set-Cookie：设置于页面关联的cookie，将其传给客户端

Server：服务器的一些信息

Access-Control-Allow-Origin：服务器端允许请求Origin头部

##### 缓存

+ 强缓存
  + Expires，时间戳
  + Cache-Content
    + 可缓存性：no-cache 协商缓存验证、no-store 不使用任何缓存
    + 到期：max-age 存储的最大周期/秒，相对于请求时间
    + 重新验证/重新加载：must-revalidate：一旦资源过期，在成功向原始服务器验证之前，不能使用
+ 协商缓存
  + Etag/If-None-Match：资源特定版本的标识符
  + Last-Modified/If-Modified-Since：最后最该时间

<img src = 'https://i.bmp.ovh/imgs/2022/01/b367c028a88374dc.png' style="zoom:67%;" />

##### cookie

Name=value

Expires=Date：有效期，缺省时表在浏览器关闭前有效

Path=Path：限制指定cookie的发送范围的文件目录，默认为当前

Domain=domain：限制cookie生效的域名，默认为创建cookie的服务域名

secure：仅在HTTPS安全连接时，才可以发送cookie

HttpOnly：js脚本无法获得

SameSite=[None|Strict|Lax]：None同站、跨站请求都可以发送、Strict 仅在同站发送、lax允许与顶级导航一起发送，并将与第三方网站发起的GET请求一起发送

#### + empty line

#### + body

### 发展

#### HTTP2 更快、更稳定、更简单

帧 frame：HTTP/2 通信的最小单位，每个帧都包含帧头，至少会标识出当前帧所属的数据流

+ 二进制

消息：与逻辑请求或响应消息对应的完整的一系列帧

数据流：已建立的连接内的双向字节流，可以承载一条或多条消息

+ 交错发送，接收方重组织

+ HTTP/2 连接都是永久的，而且仅需要每个来源一个连接
+ 流控制：组织发送方 向 接收方 发送大量数据的机制
+ 服务器推送

#### HTTPS 概述

+ HTTPS：Hypertext Transfer Protocol Secure

+ 经过 TSL/SSL加密
+ 对称加密：加解密都是使用同一个密钥
+ 非对称加密：需要是哦那个两个不同的密钥，公钥 public key、私钥 private key

## 3. 常见场景：静态资源、登陆

#### 静态资源 — 今日头条 index.css

状态码200就一定是发起请求了吗？

从Response中观察 缓存策略、资源类型、允许访问的域名

静态资源方案：缓存+CDN+文件打包时产生的hash

#### 登录

为什么有options的请求？ – cross-origin

cors：复杂请求会进行预请求，获知服务器端是否允许该跨源请求

> 相关协议头：
>
> Access-Control-Allow-Origin、
>
> Access-Control-Expose-Headers
>
> Access-Control-Max-Age
>
> Access-Control-Allow-Credentials
>
> Access-Control-Allow-Methods
>
> Access-Control-Allow-Headers
>
> Access-Control-Request-Method
>
> Access-Control-Request-Method
>
> Access-Control-Request-Headers
>
> Origin

跨域的解决方案：cors、代理服务器、iframe、jsonp…

从Response 和 Request 观察 向什么地址做了什么动作、携带了/返回了什么信息

下一次进入页面为什么能记住登陆态？ — 鉴权 session+cookie 、JWT json web token

进入同个网站的其他站点为什么也有登录态？— SSO 单点登录

## 4. 实际应用：浏览器与node中使用

### 浏览器

#### + AJAX 之 XHR

+ XHR：XMLHttpRequest
+ readyState：
  + 0 UNSENT 代理被创建
  + 1 OPENED open() 已被调用
  + 2 Header-received send() 已经被调用，获得头部和状态
  + 3 loading 下载中，responseText 属性已有部分数据
  + 4 done 下载已完成

#### + AJAX 之 Fetch

+ XHLHttpRequest 的升级版
+ 使用 Promise
+ 模块化设计，Response，Request、Header 对象
+ 通过数据流处理对象，支持分块读取

### node

标准库：HTTP/HTTPS

### 实战

请求库：axios

用户体验：

+ 网络优化：http2、cdn动态加速、dns预解析、网络预连接、域名收敛/发散、压缩、https性能优化
+ 稳定性：重试机制(超时、有误)、缓存、数据安全(Https、劫持)



## 5.了解更多：不止HTTP协议一个选择

### WebSocket

+ 浏览器与服务器进行全双工通讯的网络技术
+ URL使用ws:// 或 wss:// 等开头
+ 场景：实时性要求高、聊天室

### QUIC

基于http3，udp，目前还是草案状态