---
title: nodejs 学习笔记
date: 2021/8/16
updated: 2021/8/23
tags: 
 - nodejs
 - http
category:
 - learningNotes
---

## 0.其他知识点

### 0.1 代码风格

- JavaScript Standard Style

- Airbnb JavaScript style

- if 使用 五分号风格 只要注意在 当每一行是以 ( [ ` 这3个开头时 前要补空格

  >  ``打印字符 es6 模板字符串 支持 换行 方便拼接

快捷键：选择长度不等 `alt 选 放开 ctrl + 向右`

​             选相同的：`先选中 然后 ctrl + d` 


### 0.2 浏览器收到html响应的解析过程

从上到下依次解析，当在解析的过程中，如果发现有：

link、script、img、iframe、video、audio 等

带有 src 或者 href(除 a ) 这种具有外链的资源 属性的标签时

浏览器会自动对这些资源发起新的请求



### 0.3 相对路径的 ./

+ 文件操作中的相对路径可以省略 ./

  **使用的所有文件操作的 api 都是异步的**

  fs.readFile('data/a.txt', …)

+ 在模块加载中 相对路径不能省略 ./

  require('./data/foo.js') 

> if 只有 / 则会找到 根目录 'C: …' 绝对路径 



### 0.4 修改完代码自动重启服务器

使用第三方命令行工具 nodemon 解决

其是 基于 node.js 开发的第三方工具 需要全局安装

1. 安装 ：`npm install --global nodemon`

2. 使用 ：`nodemon app.js` 用nodemon 代替 node 启动文件

   > 会监视文件变化 然后自动重启服务器



### 0.5 <font color='red'>回调函数:获取异步操作的结果</font>

**如果需要获取一个函数中异步操作的结果，则必须通过回调函数来获取**

> 学node的精华所在 封装异步 api 5.6中有案例

~~~js
function fn(callback){
    //callback = function(data){ console.log(data) }
    setTimeout(function(){
        let data = 'hello';
        callback(data);
    },1000)
}

// 外部得到 data
fn(function(data){
    console.log(data);
})
~~~



### 0.6 npm package.js 

1. npm

   node package manager

2. package.js

   包描述文件 (项目说明书)

   npm 下载东西的时候 加 - - save (保存项目第三方包的依赖信息 dependencies)

   可以在终端 npm init 初始化项目 创建

+ 当把node_mouldes 文件夹删除后 因为有 package.js 中的依赖信息 所以**直接终端 npm install** 就会重新下回来



#### 0.6.1 npm 网站

[官方网站](https://npmjs.com)

第三方包 在哪来的 可以搜索下载上传第三方包 

#### 0.6.2 npm 命令行工具

npm 第二层含义就是 命令行工具 只要安装node就安装了npm

1. 查版本号 : `npm --version`
2. 升级 npm : `npm install --global npm`

#### 0.6.3 常用命令

+ 生成项目 : `npm init`

  + `npm init --yes` 跳过向导 快速生成

+ 下载第三方包 : `npm install`  `npm install 包名`  `npm install --save`

+ 删除包 : `npm uninstall 包` 只删除 if有依赖项就会保存

  ​              `npm uninstall --save 包`  删除包 以及其依赖项

+ 查看使用帮助 : `npm --help`

+ 查看指定命令使用帮助 : `npm 命令 --help`

+ 查看npm配置信息 : `npm config list`

#### 0.6.4 解决 npm 被墙问题

使用淘宝镜像 `cnpm`

>  npm 服务器在国外 

1. 安装 cnpm `npm install --global cnpm`
2. `cnpm`直接替换 命令 `npm`

if 不想下cnpm 可以每次这样使用 `npm install jquery --registry=https://registry.npm.taobao.org`

也可以将`npm config set registry=https://registry.npm.taobao.org` 这个配置到文件中 每次 npm就会使用cnpm



### 0.7 package-lock.json

npm 5 后，在安装包的时候，npm就会生成或者更新 `package-lock.json`这个文件

+ npm5 后安装包 不需要加 `--save` 都会自动保存依赖信息
+ 当安装包时 会自动创建或更新这个文件 (在项目的根目录下)
+ 改文件会保存 `node_modules` 中所有包的信息(版本、下载地址)
  + 这样的话 重新 `npm install` 时 速度可以提升
+ lock -> 锁
  + 锁定版本： 正常来说 项目依赖了 1.1.1,在重新下载时 会下载最新版本,但希望可以锁住版本，改文件就可以锁住版本号，防止自动升级



### 0.8 find 和 findIndex 的原理

es6 新增的方法

> 接收一个方法作为参数 方法内有一个返回条件 
>
> find 会遍历所有元素 执行给的带有条件返回值的函数
>
> if 符合改条件的元素会作为find方法的返回值
>
> if 无符号 就返回 undefined

find 和 findIndex 原理：

~~~js
const users = [ {id:1,name:'lili'}, {id:2,name:'lii'}, {id:3,name:'li'}]

Array.prototype.myFind = function(conditionFunc){
    //conditionFunc = function(item index){ return item.id === 2}
    for(let i = 0; i < this.length; i++ ){
        if( conditionFunc(this[i],i) ){  //this[i] = item ，i= index
            return this[i];   //if return i 就是 findIndex
        }
    }
}

const ret = users.myFind(function(item,index){
    return item.id === 2;
})

console.log(ret);  //{id:2,name:'lii'}

//使用
arr.find(function(item){
    return item.id === id
})
~~~



### 0.9 reduce

[1,2,3].reduce( (prev,curr) => { return prev + curr} )  //6相加

## 1. node.js介绍

### 1.1 能做什么

+ web 服务器后台
+ 命令行工具
  + npm - node
  + git - c
  + hexo - node
  + ...

> 前端接触最多的就是命令行工具，主要是用第三方的
>
> + webpack
> + gulp
> + npm



### 1.2 预备知识

html -> css -> js -> 简单命令行操作 -> 服务端开发经验更好



### 1.3 资源

+ <深入浅出 node.js>
  
  + 偏理论，无实战内容
  + 对理解底层有帮助
  + 可结合课程看
  
+ <node.js 权威指南>
  + api 讲解
  + 无实战
  
+ JavaScript 标准参考教程(alpha) : http://javascript.ruanyifeng.com/

+ node 入门 : https://www.nodebeginner.org/index-zh-cn.html

+ 官方 api 文档 ：https://nodejs.org/dist/latest-v6.x/docs/api/

+ 中文文档 (版本较旧) ：http://www.nodeclass.com/api/node.html

+ CNODE 社区 ：https://cnodejs.org

+ CNODE - 新手入门 ：http://cnodejs.org/getstart

  > es6 ： <ECMAScript 6 入门> —阮一峰
  >
  > ​           <深入理解 ES6>  — 	尼古拉斯



### 1.4 这门课程能学到的东西

+ b/s 编程模型

  + Browser - Server

  + back - end

    > 任何服务端技术 的 bs 编程模型都是一样的，和语言无关
    >
    > node 只是作为学习 bs 编程模型的一个工具

+ 模块化编程
  + RequireJS
  + SeaJS
  + `@import('文件路径')`
+ node 常用 api
+ 异步编程
  + 回调函数
  + Promise
  + async
  + generator
+ Express Web开发框架
+ Ecmascript6
  + 课程中穿插讲解
+ …



## 2. 起步

### 2.1 安装 node 环境

+ 查看版本号
+ 下载： 官网 -> 安装 -> 确认安装是否成功 -> 环境变量



### 2.2 node 执行 js 文件

1. cmd 打开到 js 文件所在的文件夹
2. 使用 ` node 文件名.js `运行 js 文件 

> 文件名 不要用 node.js
>
> if 用这个  node node.js 就会打开文件  
>
> 最好不使用中文



### 2.3 node 特点(与浏览器相比)

#### 2.3.1 ecmascript

+ 解析执行JavaScript

+ 无 dom bom

  > window document is not defined

  

#### 2.3.2 核心模块

在使用前都需要 const mondel = require('模块名称')

[官网api](http://nodejs.cn/api/) 可以看到核心模块

常用 ： fs os path request http

例子：

+ #### 可以读写文件

  + 读：

    1.使用requirt 加载 fs 核心模块 

    2.使用fs.readFile 读取文件

    ~~~js
    //第一个参数 读取的文件路径
    //第二个参数  回调函数      => error  读失败 error错误对象; 读成功 error=null
    //                          data   读失败 error错误对象; 读成功 data 读取到的数据
    
    // 因为数据 是 二进制 转为 16进制 的 ，如果要看懂的话 要用toString
    const fs = require("fs");
    fs.readFile('../nodejs.md', function (error, data) {
      // console.log(data);
      console.log(data.toString());
    });
    ~~~

  + 写：

    1.使用requirt 加载 fs 核心模块 

    2.使用fs.writeFile 读取文件

    > 要自己创建文件夹

    ~~~js
    // 第一个参数 文件路径
    // 第二个参数 文件内容
    // 第三个参数 回调函数  其参数error => 成功 error=null 失败 error=错误对象
    const fs = require("fs");
    fs.writeFile('./data/01writefile', '2021.08.16 to learn file', function (error) {
      if (error) {
        console.log("ok 200");
      } else {
        console.log(error);
      }
    })
    ~~~

+ #### http 

  1. 使用node非常轻松的构建一个web服务器

     在node中专门提供了一个核心模块：http - 职责 就是帮你创建编写服务器的

     1.加载核心模块 http

     2.使用http.createServer() 创建一个web服务器 返回一个Server实例

     3.服务器的用处 提供对数据的服务 -> 发请求 -> 接收请求 -> 处理请求 -> 发送响应

       注册request请求事件 当客户端请求过来 会自动触发服务器的request请求事件，执行回调函数

       回调函数有两个对象参数 request，response

     + requset.url 会把http://127.0.0.1:3000 后面的东西返回到终端 只要每次触发request就会

     + response.write() 会把括号内的 字符串 响应到客户端 可以用多次 但是最后要加response.end()

       + 也可以直接在 response.end(“响应数据”) **响应数据只能是二进制数据 或者 字符串**

     + 可以利用 不同的 request.url 响应不同的 write内容到客户端

     + response.setHeader('Content-Type', 'text/plain;charset=utf-8');  **解决乱码**

        在http 协议中 Content-Type 是 数据内容的类型  [oschina 网站 查表](https://tool.oschina.net/commons) 

        text/plain：普通文本 ； text/html：html ；image/jpeg：jpg (不用指定编码 只有**字符数据要**)；

     4.绑定端口号，启动服务器

     ~~~js
     const http = require("http");
     let server = http.createServer();
     server.on('request', function (request, response) {
         
       // 解决乱码问题 浏览器在不知道什么编码时 会用操作系统的默认编码 gbk
       response.setHeader('Content-Type', 'text/plain;charset=utf-8');
         
       console.log("200 ok + url: " + request.url);
       if (request.url == '/') {
         response.write('index');
       } else {
         response.write('other');
       }
       response.end()
     })
     server.listen(3000, function () {
         //因为启动需要时间 所以整一个回调函数
       console.log("服务器启动成功，可以通过http://127.0.0.1:3000/ 进行访问");
     })
     ~~~

     > 此时在终端运行文件后 会打印listen的内容 表启动了服务器 此时 如果用浏览器打开 访问 就会触发request请求
     >
     > 可以用 crtl + c 结束服务器
     >
     > <img src='https://s1.328888.xyz/2022/04/09/XK83v.jpg' style="zoom:80%; float:left;" >
     >
     > 
     >
     > 加上request 和 response 后
     >
     > <img src='https://s1.328888.xyz/2022/04/09/XKAE0.jpg' style="zoom:80%; float:left;" >

     

  2. 利用 服务器的 response 响应页面

     > 与 fs 核心模块 配合使用 到对应的url 就读相应文件 然后响应到客户端上

     ~~~js
     const http = require("http");
     const fs = require("fs");
     
     let server = http.createServer();
     server.on('request', function (request, response) {
       response.setHeader("Content-Type", "text/plain;charset=utf-8");
       console.log("访问路径为: " + request.url);
       if (request.url == '/index') {
         fs.readFile('./view/learn.html', function (err, data) {
           if (err) {
             console.log("文件读取失败 请稍后重试");
           } else {
             response.setHeader("Content-Type", "text/html;charset=utf-8");
             response.end(data)
           }
         })
       } else {
         response.end("其他")
       }
     
     })
     
     server.listen(3000, function () {
       console.log("服务器启动成功，可以通过http://127.0.0.1:3000/ 进行访问");
     })
     ~~~

     



#### 2.3.3 第三方模块

#### 2.3.4 用户自定义模块

> node 中只有 模块作用域

require 

作用 ：用来加载模块执行代码 (1.具名的核心模块 fs.. 2.用户自己编写的文件模块 ./… ./不能省略 后缀名可以省 )

​                 拿到模块中的导出对象 





### 2.4 ip地址和端口号

+ 网卡：只有一个 同一个局域网中 网卡的地址唯一 通过唯一的ip地址来进行定位
+ ip 地址 用来定位计算机 
+ 端口号 用来定位具体的应用程序
  + 所有需要联网的应该程序都会占用一个端口号
  + 端口号的范围 0 - 65536 之间
  + 不要用到默认端口号 

> 核心模块 http 中  const http = require('http') 
>
> 创建server let server = http.createServer()
>
> 监听request 请求事件 server.on('request',function(request , response){…})
>
> + 请求url : request.url
> + 请求我的客户端端口号 : request.socket.remotePort
> + 请求我的客户端地址 ip+port : request.socket.remoteAddress 



### 2.5 服务端渲染 和 客户端渲染

1. 服务端渲染

   + **页面刷新** 服务端渲染 网页源码有
   + 只请求一次
   + 响应的就是页面最终结果

   + <img src='https://s1.328888.xyz/2022/04/09/XKHRF.jpg' style="zoom:60%;" >

2. 客户端渲染

   + **点击页面不刷新** ajax 异步请求 数据信息在开发者工具才能看到
   + 两次请求
   + 第一次 拿到页面
   + 第二次 拿到动态数据
   + 再将数据渲染到页面上
   + <img src='https://s1.328888.xyz/2022/04/09/XKq6W.jpg' style="zoom:60%;" >



### 2.5 REPL

在终端直接输入node 回车 就可以测试 node 代码

+ read 
+ eval
+ print
+ loop



## 3. 模板引擎

> to 字符串替换

[art-template 官网](https://github.com/aui/art-template) 



### 3.1 安装

~~~shell
npm install art-template --save
~~~



### 3.2 在 html 浏览器中使用

模板引擎不关心 字符串的内容 只关心自己认识的模板标记语法 例如 mustache 语法 `{{}}`

1. 安装
2. 导入 lib/template-web.js
3. 写 text/template id='bbb' 的模板
4. 写 `template('bbb'{…})` 即模板的`{{}}` 里的数据

~~~html
<body>
  <script src="node_modules/art-template/lib/template-web.js"></script>
  <script type="text/template" id='tpl'>
    我叫{{name}}
  </script>
  <script>
    const ret = template('tpl', {
      name: "lsy"
    })
    console.log(ret); //模板引擎里面的内容
  </script>
</body>
~~~



### 3.3 在 node 中使用

> 模板最早诞生于服务器领域 后来才发展到了前端

1. 安装
2. 加载 art-template
3. 查文档 使用api

~~~js
const template = require("art-template");
const str = `<p> hello 我是 {{name}} </p>`
let ret = template.render(str, {
  name: 'lsy'
})
console.log(ret);
~~~

将模板作为html 导入的写法 并且替换html 响应到页面上

~~~html
//./02-data.html
...
<body>
  <p>hello 我是{{name}}</p>
</body>
...
~~~

~~~js
const template = require("art-template");
const fs = require("fs");
const http = require("http");
let server = http.createServer();
server.on('request', function (request, response) {
  fs.readFile('./02-data.html', function (err, data) {
    if (err) {
      return console.log("文件读取失败");
    }

    let ret = template.render(data.toString(), {
      name: 'lsy'
    })
    // console.log(ret);
    response.end(ret)
  })
})
server.listen('3000', function () {
  console.log('running...');
})
~~~



### 3.4 发表留言的例子

#### 3.4.1 目录结构：

' > node_modules : 

' > public : 把所有静态资源都放在这 

​             ' > css

​             ' > img

​             ' > js

​             ' > lib : jquery.js 这种第三方文件

' > views : 所有 html 文件

' > app.js : 后端业务

#### 3.4.2 表单中需要提交的表单控件元素 必须具有 **name** 属性

表单提交分为：1.默认的提交行为 2.表单异步提交

action 表单提交的地址 == 请求的 url

method 请求方法

#### 3.4.3 url 核心模块

url.parse('…url…' ，true) : 将…url… 解析为一个方便接受的对象 ；

​                                        第二个参数==true时，会将查询字符串 query 拆成对象 否则为字符串  

#### 3.4.4 重定向

1. 状态码设置为 302 临时重定向

2. 在响应头通过 Location 通知客户端重定向的地址  

3. res.statusCode = 302;

     res.setHeader('Location', '/');

     res.end();

> 如果客户端 发现服务器的响应的状态码是302 就会去自动找响应头中找 Location 对应的url
>
> 所以就可以看到客户端自动跳转



#### 3.4.5 each

1. art-template 的专属each 模板语法

   ~~~html
   {{each 数组}}
   <li>{{ $value }}</li>
   {{/each}}
   ~~~

2. es5 的 forEach

   ~~~js
   //ie 8 不支持
   ['abc','d','efg'].forEach(function(item,index){
       
   })
   ~~~

3. 遍历 jq 元素

   ~~~js
   //jq 2.0 以下 可以兼容ie 低版本
   $.each(['abc','d','efg'],function(index,item){
       
   })
   ~~~

4. 伪数组的遍历 eg：$('div')

   伪数组是对象  对象的原型链Object.prototype 中没有forEach   不能用 如果要用的话 看下面

   ~~~js
   //这个 each 是 jq 提供的 是在 jq 的原型链中的
   $('div').each(function(){})
   //或者可以用forEach
   [].slice.call($('div')).forEach(function(){})
   ~~~

   



## 4. Node 中的模块系统

使用 node 编写应用程序 主要就是在使用：

1. EcmaScript 语言
2. 核心模块
3. 第三方模块
4. 自定义模块



### 4.1 模块化

+ 文件作用域

+ 通信规则
  + 加载 require
  + 导出

### 4.2 CommonJS 模块规范

+ 模块作用域

+ 加载模块 require

  ~~~js
  require('文件url')
  ~~~

+ 导出模块成员 exports

  ~~~js
  //导出多个成员
  export.a = '...';
  export.b = '...';
  ...
  //导出单个成员 后面会覆盖前面
  module.exports = '...'
  ~~~



原理：

~~~js
//里面有隐藏的
let module = {
    exports:{
        ...
    }
};
let exports = module.exports;
return module.exports; 

//导出单个成员 不能export = '...' 这样就会指向另一个对象 导出的数据就不对了
// 即 给 exports 和 module.exports 赋值都会断开引用
~~~



### 4.3 require 加载机制

优先从缓存加载 -> 核心模块 -> 路径形式的文件模块 -> 第三方模块…

> 更加底层的在 《深入浅出node.js》模块化

require('模块标识符')  : 核心模块/第三方模块/自定义模块

if 第三方模块

1. 通过 npm 下载

2. 使用时 通过 require(“包名”) 进行加载

3. 不可能有一个第三方包与核心模块的名字相同

4. 既不是核心模块 也不是路径的时候 会 (eg：art-template)

   + 找到当前文件所在目录中的 node_modules 目录

   + 再依次找到 node_modules/art-template/package.json 文件中的 main(: 'index.js') 属性

     > if 没有 package.json 或者没有main ,就会自动找 index.js (默认备选项)

   + main 属性中就记录了 art-template 的入口模块

   + 然后加载使用这个第三方包 实际上最终加载的还是文件

   + if 本级无法查找到 就会往上一级 找node_modules… 直到根目录

   + if 还无 就报错

   + 正常项目就一个node….在根目录
   
   

## 5. Express

> 原生 http 在某方面表现不足以应对开发需求 所以需要使用框架 加快开发效率
>
> 在node中 有很多web开发框架 以学习Express(封装http)为主
>
> 作者 : TJ  node作者:ryan dahl

[官方网站](https://expressjs.com/)

### 5.1 起步

#### 5.1.1 **安装**

`npm install express --save`

#### 5.1.2 **使用**：

1.导包 2.创建服务器应用程序

> + 处理路径问题 和中文乱码 app.get('/', function (req, res) { res.send('hello')  })
> + 公开指定目录 app.use('/public/', express.static('./public/'))
> + 直接获取查询字符串参数 req.query
> + 响应代码 res.send() 
> + res.redirect('/') 和 res.send() 会直接结束请求

~~~js
// 2. 引包
const express = require("express");

// 3. 创建服务器应用程序 == http.createServer
let app = express();

// 公开指定目录 public 就可以直接通过 /public/xxx 访问目录中的所有资源
app.use('/public/', express.static('./public/'))

// 服务器收到get请求时 执行回调函数
// 直接处理路径问题 和中文乱码
app.get('/', function (req, res) {
  res.send('hello')   // 也可以原来的 res.end()
})
app.get('/other', function (req, res) {
  console.log(req.query); // 直接获取 查询字符串参数 
  res.send('其他')
})

// ==server.listen
app.listen(3000, function () {
  console.log('running...');
})
~~~



#### 5.1.3 基本路由

路由器：请求方法 请求路径 请求处理函数

app.get('/',function(){})

app.post('/',function(){})



#### 5.1.4 静态服务/路由 

[官网 -> getting started -> static file]()

1. 当以 /public/ 开头时 去 ./public/ 目录找对应的资源 

   app.use('/public/' , express.static('./public/'))  => /public/xxx 

2. 当省略第一个参数时 则通过省略 /public 的目录 访问 简化路径操作

   app.use(express.static('./public/')) => /xxx

3. 当第一个参数 为 /a/ 时 ，相当于 在目录中 ./public/ 被 ./a/ 替换(别名)

   app.use('/a/' , express.static('./public/'))  => /a/xxx



### 5.2 express 配置使用 art-template 模板引擎

>1. 生成 package.js ==> `npm init –yes`
>2. 安装 express ==> `npm i -S express`

1. 安装在express 使用的art-template

   ==> `npm i -S art-template`  `npm i -S express-art-template`

2. 配置：

   …

   app.engine('html', require('express-art-template'))  ==> 'art' 可以 'html'

   …

   > 第一个参数：当渲染以 .art 文件时 使用art-template模板
   >
   > express-art-template 专门用来在 express 中把 art-template 整合到 express 中
   >
   > express-art-template 依赖 art-template

3. 使用：

   app.get('/',function(req,res){ res.render('index.art'),{ title:'hello' } })

   > Express 为 Response 对象提供了 render 方法 默认不能用 配置模板引擎才能用
   >
   > res.render('html模板名' , {模板数据})  ==> **if 上engine 为 art 那模板名就要.art**
   >
   > 第一个参数不写路径(**省略 views 而已**) 默认会去 views 目录找 模板文件 (把所有视图文件都放到 views )

   > if 希望修改 默认 的 views 视图渲染目录 ==> app.set('views',目录路径)

   

### 5.4 express 获取表单 get 请求体数据

express 内置了api 直接通过 req.juery 获取



### 5.5 express 获取表单 post 请求体数据

提交表单 get —> post  /puton —> publish  : 用同一个请求路径 可以多次处理请求 get/post

在express 中没有内置获取表单post请求体的api 

==> 使用 第三方包 `body-parse` (express官网 minddleware )中间件/插件 

1. 安装 : `npm i -S body-parse`

2. 配置 : 

   ~~~js
   const express = require('express');
   const bodyParser = require('body-parser')          //---引包
   
   var app = express();
   
   // 在req请求多一个body属性  通过req.body 获取表单post请求体数据
   app.use(bodyParser.json())                         //---配置 body-parser
   app.use(bodyParser.urlencoded({ extended: true }))
   
   app.post('/publish', function (req, res) {
     comments.unshift(req.body);
     res.redirect('/')
   })
   ~~~

     

### 5.6 crub 案例的知识点

> 注意点 ：表单提交的时候 要有name 才能提交到 查询字符串

+ 把对象存在文件里 然后读取使用 读取的是字符串->对象

  读取文件错误 用状态码500

  ~~~js
  fs.readfile('./db.json',function(err,data){
      if(err){
          return res.status(500).send('server error');
      }
      student:JSON.parse(data).students
  })
  ~~~

  

+ 路由设计

  请求方法、请求路径、get参数、post参数、备注

+ 将 基本路由与app主要的东西分离开来 把app导出

  运行 app.js 文件

  ~~~js
  //app.js
  const express = require("express");
  const router = require('./router');  //!!!
  
  let app = express();
  
  app.use('/public/', express.static('./public/'))
  
  router(app);    //!!!
  
  app.listen(3000, function () {
    console.log('running....');
  })
  
  module.export = app;
  
  //router.js
  module.exports = function(app){
      app.get('/',...)
  }
  ~~~

+ express 专门包装路由的方式

  > app.js 职责：创建服务 
  >
  > ​                    做一些服务相关配置:模板引擎、body-parser解析表单 post 请求体、提供静态资源服务
  >
  > ​                     挂载路由
  >
  > ​                     监听端口启动服务
  >
  > router.js 职责：处理路由 根据不同的请求方法+路径 具体处理函数

  1. 创建路由容器 router
  2. 把路由都挂载到 router 路由容器中
  3. 导出 router
  4. 把路由挂载到app服务器上

  ~~~js
  //app.js
  const express = require("express");
  const router = require('./router');  //!!!
  
  let app = express();
  
  app.use('/public/', express.static('./public/'))
  
  app.use(router);    //挂载!!!
  
  app.listen(3000, function () {
    console.log('running....');
  })
  
  module.export = app;
  
  //router.js
  const express = require("express");
  const router = express.Router();
  router.get('/',...);
  router.get('/',...);
  module.exports = router
  ~~~



## 6. MongoDB

[教程](https://www.runoob.com/mongodb/nosql.html)

### 6.1 关系型数据库 和 非关系型数据库

+ 关系 === 表 (表与表之间存在关系)

  > 需要通过 sql 语言操作
  >
  > 操作之前要设计表结构
  >
  > 数据包支持约束

+ 非关系

  > 非常灵活
  >
  > 有的即使 key-value 键值对

  MongoDB 是长得最像 关系型数据库的 非关系型数据库

  + 数据库 -> 数据库
  + 数据表 -> 集合/数组
  + 表记录 -> 文档对象
  + 不需要设计表结构 可以任意存数据 没有结构性 比较灵活

### 6.2 安装 

[下载](https://www.mongodb.com/try/download/community)

安装

配环境变量 : 命令行检查 `mongod --version`



### 6.3 启动、关闭、连接、退出连接数据库

+ 启动：`mongod`

​        mongod 默认使用执行 mongod 命令所处盘符根目录下的 /data/db 作为自己的数据存储目录

​        第一次执行时应该提前创建一个 /data/db

+ 修改默认的数据存储目录 : `mongod --dbpath=数据存储目录路径`

+ 关闭 : ctrl + c / 关闭终端

+ 连接 ： mongo (默认连接本机的 MongoDB 服务)

+ 断开 ： exit (退出连接)



### 6.4 基本命令

+ `show dbs ` : 查看显示所有数据库
+ ` db ` : 查看当前操作的数据库
+ ` use 数据库名称 ` : 切换到指定的数据库(if 本无 —> 创建后切换)
+ `db.集合名称.insertOne({...})` : 插入数据 —> 一个对象
+ `db.集合名称.find()` : 查看这个集合的所有文档对象



### 6.5 在node中 操作 mongoDB 数据

+ 通过官方 `mongodb` 包操作 比较原生 一般不用

  [教程](https://github.com/mongodb/node-mongodb-native)

+ 通过第三方包 `mogoose` 操作MongoDB 数据库

  基于官方(上面)做的一个封装 [官网](https://mongoosejs.com/)

  1. 下载包 `npm install mongodb --global`

  2. 下载包 `npm install mongoose --save`

  3. 写一个官网的例子

     ~~~js
     const mongoose = require('mongoose');
     // 连接MongoDB数据库
     mongoose.connect('mongodb://localhost:27017/test', { useNewUrlParser: true, useUnifiedTopology: true });
     
     // 创建一个模型 设计数据库 Mongodb 是动态的 只需要在代码中设计数据库
     // mongoose 这个包可以让你的设计编写过程变得非常简单
     const Cat = mongoose.model('Cat', { name: String });
     
     // 实例化一个cat
     const kitty = new Cat({ name: 'Zildjian' });
     
     // 持久化保存 kitty实例
     kitty.save().then(() => console.log('meow'));
     ~~~

  4. 在 终端 运行 `node demo.js`

  5. 可以在数据库查看

     <img src='https://s1.328888.xyz/2022/04/09/XKlCk.jpg' style="zoom:77%;" >

     

#### 6.5.1 mongoose

+ 官网：https://mongoosejs.com/
+ 官方指南：https://mongoosejs.com/docs/guide.html
+ 官方api：https://mongoosejs.com/docs/api.html

#### 6.5.2 MongoDB 数据库的基本概念

+ 数据库

+ 集合

+ 文档 -> 文档结构没有限制

+ 很灵活 

  > 在需要插入数据时 只需要指定往哪个数据库的哪个集合操作就可以了
  >
  > 其他都有 mongodb 自动完成建库建表

~~~js
{
    qq:{                          //数据库
        users:[                   // 集合 ->数组
            {name:'li', age:15},  //文档 -> 表记录 对象
        ]
    }
}
~~~



### 6.6 官方指南

#### 6.6.1 设计Scheme 发布 Model

~~~js
const mongoose = require('mongoose');

const Schema = mongoose.Schema; //创建设计结构

// 1.连接数据库
// 指定连接的数据库不需要存在，在插入第一条数据之后就会自动被创建处理
mongoose.connect('mongodb://localhost/itcast')

// 2.设计集合/文档结构
// 字段名称就是表结构中的属性名称
// 约束的目的是为了保证数据的完整性 避免脏数据

const userSchame = new Schema({
  username: {
    type: String,
    require: true
  },
  password: {
    type: String,
    require: true
  },
  email: {
    type: String
  }
})

// 3.讲文档结构发布为模型
//   mongoose.model 方法就是用来将一个架构发布为 model
//   第一个参数：传入一个大写名称单数字符串  User -> 数据库名称
//               mongoose 会自动将大写名词的字符串 生成 小写复数  users ->  集合名称
//   第二个参数：架构 Schema
//   返回值：模型对象/构造函数
const User = mongoose.model('User', userSchame)


// 4. 当有了模型构造函数后 就可以使用构造函数对 users集合 中数据形象操作
~~~



#### 6.6.2 增加数据

~~~js
const admin = new User({
  username: 'admin',
  password: '123456',
  email: 'admin@admin.com'
})

admin.save(function (err, ret) {
  if (err) {
    console.log('err');
  } else {
    console.log('success');
    console.log(ret);
  }
})

//console结果
{
  _id: 612124d035f7324bb036ea01,
  username: 'admin',
  password: '123456',
  email: 'admin@admin.com',
  __v: 0
}
~~~



#### 6.6.3 查询数据

查到的数据都会放在 数组里[]

查不到就是 null

1. 查询所有

   ~~~js
   User.find(function (err, ret) {
     if (err) {
       console.log('err');
     } else {
       console.log('success');
       console.log(ret);
     }
   })
   //ret
   [
     {
       _id: 612124d035f7324bb036ea01,
       username: 'admin',
       password: '123456',
       email: 'admin@admin.com',
       __v: 0
     }
   ]
   
   ~~~

2. 条件查找

   ~~~js
   User.find({ username: 'zs' }, function (err, ret) {
     if (err) {
       console.log('err');
     } else {
       console.log('success');
       console.log(ret);
     }
   })
   
   //ret
   [
     {
       _id: 612126bb120a184418be6b6d,
       username: 'zs',
       password: '123456',
       email: 'admin@admin.com',
       __v: 0
     }
   ]
   ~~~

3. 查询单个 if无条件 查第一个  **&  条件**

   ~~~js
   User.findOne({ username: 'zs', password:'123456' }, function (err, ret) {
       // & 条件 
    if (err) {
     console.log('err');
    } else {
     console.log('success');
     console.log(ret);
    }
   })
   ~~~

4. **|| 条件**查询  **$or:[]**

   ~~~js
   User.findOne(
       { $or: [{username: 'zs'}, {password:'123456'}] },
       function (err, ret) {
       // & 条件 
            if (err) {
             console.log('err');
            } else {
             console.log('success');
             console.log(ret);
        }
   })
   ~~~

   

#### 6.6.4 删除数据

~~~js
User.remove({ username: 'zs' }, function (err, ret) {
  if (err) {
    console.log('err');
  } else {
    console.log('success');
    console.log(ret);
  }
})

//ret
{ n: 1, ok: 1, deletedCount: 1 }


// 删除 根据条件删除所有
User.findOneAndRemove({ conditions }, [options], [callback])

// 删除 根据id删除一个
User.findByIdAndRemove({ conditions }, [options], [callback])

~~~



#### 6.6.5 更新数据

第一个参数：id 之前控制 ret 出现的 id
       第二个参数：修改的值

~~~js
// 第一个参数：id 之前控制 ret 出现的 id
// 第二个参数：修改的值
User.findByIdAndUpdate('612124d035f7324bb036ea01', { password: '22222' }, function (err, ret) {
  if (err) {
    console.log('err');
  } else {
    console.log('success');
    console.log(ret);
  }
})
~~~



### 6.7 mongoose 的 api 全部支持 promise

#### 6.7.1 查询数据

~~~js
// promise 查询所有
User.find().then(data => console.log(data))

// 做一个用户注册案例 查找用户是否存在
User.findOne({ name: 'lsysxn' }).then(user => {
  if (user) {
    // 用户存在
    console.log('always exist');
  } else {
    return new User({
      username: 'lsysxn',
      password: '123456',
      email: 'lsy'
    }).save()
  }
}).then(ret => { })
~~~



## 7. 论坛项目

### 7.1 path 模块

path 核心模块 — 操作路径

+ path.basename(path [, ext]) : 获取path中的文件名 无对应后缀名 [, ext] 就返回加上后缀的文件名

  > path.basename('D:/a/b/index.js')  —> index.js        path.basename('D:/a/b/index.js' , '.js')  —> index

+ path.dirname(path) : 返回path中的目录

  > path.dirname('D:/a/b/index.js')  —> D:/a/b

+  path.extname(path) : 返回 path 中的后缀名

  > path.extname('D:/a/b/index.js')  —> .js

+ path.isAbsolute(path) : 判断是否绝对路径

+ path.parse(path) : 将 path 解析成对象

  > path.parse('d:/a/b/index.js') —> { root: 'd:/' , dir: 'd:/a/b' , base: 'index.js' , ext: '.js' , name: 'html'}

+ path.join([…paths]) : 拼接 paths , 两个path之间加 /, 支持任意个参数

  > path.join('c:/a' , 'b') —> `c:\\a\\b`  2 \ 转义成一个\ windowns 表示路径是用\



### 7.2 node中的其他成员

在每个模块中，除了 `require` `exports` 等模块相关的api之外 还有两个特殊成员：

+ `__dirname` : **动态**获取当前文件模块所属目录的绝对路径
+ `__filename` : **动态**获取当前文件的绝对路径(包含 文件名)
+ `__dirname` 和 `__filename` 不受node 命令所属路径影响



在文件操作核心模块 fs 中 读取文件 里文件路径 

if  ./a.html -> 读取的是 **相对于执行node命令所处的终端路径**

> eg：a.html 在 c:/in/out/side/a.html  
>
> 在这个路径下还有一个index.js -> c:/in/out/side/index.js 有一个读文件的操作 fs.readfile('./a.html')
>
> 在终端这样运行 **c:/in/out/side**> node  **index.js** 时是不会报错的  此时读的a.html地址 **c:/in/out/side/html**
>
> 但这样运行 **c:/in/out**> node  **side/index.js** 时 读的a.html的地址就是 **c:/in/out/index/js**
>
> 所以在文件操作中，使用相对路径不可靠 ==>> 所以要使用动态的绝对路径
>
> 为了避免在使用的过程中出现路径拼接操作 使用 path.join()

所以推荐在**文件模块操作**中使用的相对路径统一转换为 **动态的绝对路径**

—> fs.readFile(path.join(__dirname , './a.txt') , 'utf8' , function(err,data){ … })

​      app.use('/public/', express.static('./public/'));   

—> app.use('/public/', express.static(path.join(__dirname , './public/')));

—> app.set('views' , path.join(__dirname , './views/'))  //默认就是 ./views 目录

—> 

(模块中的路径标识不受node命令所处的路径影响)



### 7.3 art-template 中的 include extend block 语法

> 在node 中还有很多第三方模板引擎可以使用 ejs   jade(pug)   nunjucks  ` <%%>` `{{ }}`

子模版 模板继承

1. `{{ include  '文件路径' }}`    将 文件路径的 文件导入该 html 
2. `{{ extend  '文件路径' }}`   继承 文件路径 中的模板 
3. `{{ block  'name' }}   {{ /block}}`  在模板中 留坑；在继承模板的html中填坑



也可以用 if else 选择要显示的dom

`{{  if 条件 }}`

`{{  else  }}`

`{{  /if  }}`



### 7.4 ajax 接受数据为 json 类型

1. 封装ajax  eg: 注册页面 post请求

   ~~~js
   $.ajax({
       url:'/register',
       type:'post',
       data:formData,
       dataType:'json',
       success:function(data){
           if(err_code === 0){
               log("成功")
           }else if(err_code === 1){
               log("邮箱或者密码已存在")
           }else if(err_code === 500){
               log("错误")
           }
       }
   })
   ~~~

2. 数据响应时 也要用json格式  给每一个状态设置一个状态码 好用于上面success 进行处理

   ~~~js
   ...
   if(err){
      return res.status(500).json({
          err_code:500,
          message:'服务器错误'
      }) 
   }
   if(data){
       return res.status(200).json({
           err_code:1,
           message:'邮箱或者密码已存在'
       })
   }
   res.status(200).json({
       err_code:0,
       message:'ok'
   })
   ...
   ~~~



### 7.5 密码加密 buleimp-md5

1. 下载 ：`npm i buleimp-md5`

2. 引包：`const md5 = require('buleimp-md5')`

3. 使用   给密码 进行md5 重复加密 ：`body.password = md5(md5(body.password))`

   > 在准备创建用户前进行密码加密
   >
   > 下一步
   >
   > new User(body).save()



加密后 if 要 登录 验证账号密码 要

~~~js
User.findOne({
    email:body.email,
    password:md5(md5(body.password))
},function(){})
~~~





### 7.6 表单提交行为

表单默认提交行为：同步 ==> 同步表单提交，浏览器会锁死(转圈)等待服务器响应结果

表单同步提交后，无论服务器端响应什么，都会直接将响应结果覆盖掉当前页面

===> **服务器重定向**



#### 7.6.1 服务器重定向对异步请求无效

所以上面的代码中在 这里成功的时候要 重定向

~~~js
$.ajax({
    url:'/register',
    type:'post',
    data:formData,
    dataType:'json',
    success:function(data){
        if(err_code === 0){
            window.location.href = '/'   //!!!!!!
            //log("成功")
        }else if(err_code === 1){
            log("邮箱或者密码已存在")
        }else if(err_code === 500){
            log("错误")
        }
    }
})

//在服务器这样是无效的
res.status(200).json({
    err_code:0,
    message:'ok'
})

res.redirect('/')   //无效
~~~



### 7.7 第三方中间件 express-session

cookie 用来保存一些不太敏感的数据  不能用来保存登陆状态

session 比较安全 

登陆状态 会有一个凭证  这个凭证是服务器给的 不容易伪造



在Express中 默认不支持 Session 和 Cookie

#### 7.7.1 express-session

1. 安装：`npm i  express-session`

2. 引用：`const session = require('express-session')`

3. 配置：(一定要在 app.use(router)之前)

   ~~~js
   // 会为 req 请求对象添加一个成员 req.session
   app.use(session({
       secret:'keyboard',   
       //配合字符串 会在原有加密基础之上 加上这个字符拼起来 去加密 
       //== body.password = md5(md5(body.password) + 'keyboard')
       resave:false,
       saveUninitialized:true
   }))
   ~~~

4. 使用：添加session数据  —>  req.session.foo = 'bar'

   ​           获取session数据 —>  req.session.foo

> 默认 session 数据是内存存储的 服务器一旦重启 就会丢失 真正的生产环境会把 session 进行持久化 存储



#### 7.7.2 浏览器插件editthiscookie

可以看cookie 和 ssession 的字段值



## 8. 中间件

在Express中

当请求进来，会从第一个中间件进行匹配 **:** if 匹配  —> 请求进入中间件 

​                                                                                 if 调用了next  —> 进入下一个匹配的中间件

​                                                                                 if 没调用  —>   直接结束

​                                                               if 不匹配 —> 一直往后走到匹配/结束为止

### 8.1 应用程序级别中间件

1. 万能匹配(不关心任何请求路径和请求方法)：

   ~~~js
   app.use(function(req,res,next){
       log(...)
       next()
   })
   ~~~

2. 以某路径 /xxx/ 开头

   ~~~js
   app.use('/a',function(req,res,next){
       log(...)
       next()
   })
   ~~~



### 8.2 路由级别中间件

1. get

   ~~~js
   app.get('/',function(req,res,next){
       res.send(...)
   })
   ~~~

2. post

   ~~~js
   app.post('/',function(req,res,next){
       res.send(...)
   })
   ~~~

3. …



### 8.3 错误处理中间件

~~~js
app.use(function(err,req,res,next){
    console.error(...)
    res.status(500).send(...)
})
~~~



#### 8.3.1 配置处理一个 404 的中间件

在app.use(router) 挂载路由后

~~~js
//在app.use(router) 挂载路由后
app.use(function(req,res){
    res.render('404.html')
})
~~~



#### 8.3.2 配置一个处理全局错误处理中间件

在处理404页面后

~~~js
//在处理404页面后
app.use(function(err,req,res,next){
    res.status(500).json({
        err_code:500,
        message:err.message
    })
})
~~~



在其他的请求处理中应该再加一个参数 ，以及在有错误时要这样处理 next(err)

> 这样就会往后查找 带有四个参数的应用程序级别中间件中

~~~js
app.get('/',function(req,res,next){
    fs.readFile('./a.txt',function(err,data){
        if(err){
            next(err)
        }
       ...
    })
})
~~~























