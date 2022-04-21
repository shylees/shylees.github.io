---
title: Nodejs 与前端开发实战 - 字节青训营
date: 2022/1/24
updated: 2022/1/24
comments:
tags:
 - nodejs
 - 青训营
categories:
 - learningNotes
layout:
permalink:
meta: true
---

## 1. Nodejs 的应用场景 why

#### 前端工程化

+ Bundle：webpack、vite、esbuild、parcel
+ Uglify：uglifyjs
+ Transpile：bablejs、typescript
+ 其他语言加入竞争：esbuild、parcel、prisma
+ 现状：难以替代

#### web 服务端应用

+ 学习曲线平缓，开发效率高
+ 运行效率接近常见的编译语言
+ 社区生态丰富及工具链成熟 npm、v8 inspector
+ 与前端结合的场景会有优势 ssr
+ 现状：竞争激烈，nodejs 有自己独特的优势

#### Electron 跨端桌面应用

+ 商业应用：vscode、slack、discord、zoom
+ 大型公司内的效率工具
+ 现状：大部分在选型时，都值得考虑

#### Nodejs 在字节

+ BFF、SSR
+ 服务端应用：头条搜索、西瓜视频
+ Electron：飞连、飞书



## 2. Nodejs 运行时结构 what

<img src="https://i.bmp.ovh/imgs/2022/01/1a6e5ab8fa56abba.png" style="zoom:67%;" >

> 社区npm代码 ：acron、node-inspect 调试、npm本身
>
> N-API：js性能太低，想用更native的语言
>
> v8：实现js运行时
>
> libuv：封装了操作系统api，nodejs最核心的内容
>
> nghttp2：与 http2 相关的模块
>
> zlib：做一些场景压缩解压缩的算法
>
> c-ares：做dns查询的库
>
> llhttp：做http协议的解析
>
> openssl：网络层的加密解密协议

#### V8，libuv

V8：JavaScript Runtime，诊断调试工具 inspector

libuv：eventloop 事件循环，syscall 系统调用

举例：用 node-fetch 发起请求时…

> nodejs 其实是基于 v8 做的。
>
> 面试 ：libuv是用来干嘛的？

#### 特点

+ 异步 I/O

+ 单线程 worker_thread可以起独立线程，但每个线程的模型没有太大变化

  JS 单线程：JS 线程 + uv 线程池 + V8 任务线程池 + V8 inspector 线程

  优点：不用考虑多线程状态同步问题，也就不需要锁；同时还能比较较高效地利用系统资源

  缺点：阻塞会产生更多负面影响 – 使用多进程 或者 多线程

+ 跨平台

  Nodejs 跨平台 + JS 无需编译环境 ( + web 跨平台 + 诊断工具跨平台 ) – 开发成本低、整体学习成本低



## 3. 编写 Http Server how

### 3.0 安装 nodejs

~~~js
// json
const http = require('http');

const server = http.createServer((req,res) => {
    const bufd = [];
    req.on('data',(buf) => {
        bufs.push(buf)
    })
    req.on('end',() => {
        const buf = Buffer.concat(bufs).toString('utf8');
        let msg = 'hello';
        try{
            const ret = JSON.parse(buf);
            msg = ret.msg;
        }catch(err){
            
        }
        const responseJson = {
            msg:`receive:${msg}`
        }
        res.setHeader('Content-Type', 'application/json');
        res.end(JSON.stringify(responseJson))
    })
})

const port = 3000;

server.listen(port, ()=>{
  console.log(`server listen on:${port}`);  
})
~~~



### 3.1 编写 Http Server + Client ，收发 GET，POST

~~~js
const http = require('http')

const body = JSON.stringify({
    msg: 'Hello from my own client'
})

const req = http.request('http://127.0.0.1:3000',{
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
    }
}, res => {
    const bufs = [];
    res.on('data', buf =>{
        bufs.push(buf)
    })
    res.on('end', () => {
        const buf = Buffer.concat(bufs);
        const json = JSON.parse(buf);
        console.log('json.msg is:', json.msg)
    })
})
req.end(body)
~~~



~~~js
// 用 promise + async await 重写这两例子
// 技巧：将callback 转换为 promise
// eg：改写 上上个代码
const http = require('http');

const server = http.createServer(async (req,res) => {
    const msg = await new Promise((resolve, reject) => { //change
        const bufd = [];
    	req.on('data',(buf) => {
        	bufs.push(buf)
    	})
        req.on('error', (err) => {  // change
            reject(err)             // change
        })                          // change
        req.on('end',() => {
            const buf = Buffer.concat(bufs).toString('utf8');
            let msg = 'hello';
            try{
                const ret = JSON.parse(buf);
                msg = ret.msg;
            }catch(err){

            }
            resolve(msg);   // change
        })
    })
     const responseJson = {
            msg:`receive:${msg}`
      }
      res.setHeader('Content-Type', 'application/json');
      res.end(JSON.stringify(responseJson))
})

const port = 3000;

server.listen(port, ()=>{
  console.log(`server listen on:${port}`);  
})
~~~



### 3.2 编写静态资源服务器

简单静态文件服务：

与高性能、可靠的服务相比，还差：CDN 缓存加速、分布式储存、容灾

外部服务：cloudflare，七牛云，阿里云，华山云

> 用 stream 风格的 api 有什么好处？
>
> 占用尽可能少的内存空间、内存使用率更好

~~~js
const http = require('http');
const fs = require('fs');
const path = require('path');
const url = require('url');

const folderPath = path.resolve(__dirname, './static');

const server = http.createServer((req,res) => {
    // expected http://127.0.0.1:3000/index.html
    const info = url.parse(req.url);
    // static/index.html
    const filepath = path.resolve(folderPath, './', info.path);
    // stream api..
    const filestream = fs.createReadStream(filepath);
    res.pipe(filestream)
    
})

const port = 3000;

server.listen(port, ()=>{
  console.log(`server listen on:${port}`);  
})
~~~



### 3.3 编写 React SSR

SSR — server side rendering 特点：

+ 相比传统 HTML 模板引擎：避免重复编写代码
+ 相比 SPA：首屏渲染更快，seo 友好
+ 缺点：通常qps 较低，前端代码编写时要考虑服务端渲染情况

### 3.4 适用 inspector 进行调试、诊断

+ v8 inspector：开箱即用，特性丰富强大，与前端开发一致，跨平台
  + 启动时 `node  --inspect 文件.js `
  + open http://localhost:9229/json
+ 场景：
  + 查看 console.log 内容
  + breakpoint
  + 高 cpu、死循环：cpuprofile
  + 高内存占用：heapsnapshot
  + 性能分析

### 3.5 部署简介

解决的问题：

+ 守护进程：当进程退出时，重新拉起
+ 多进程：cluster 便捷地利用多进程
+ 记录进程状态，用于诊断

容器环境：

+ 通常有健康检查的手段，只需要考虑多核 cpu 利用率即可

## 4. 延伸话题

nodejs 贡献代码

追踪/诊断

WASM，NAPI 

