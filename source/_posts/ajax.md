---
title: ajax 学习笔记
date: 2021/9/9
updated: 2021/9/10
tags: 
 - js
category:
 - learningNotes
---

## 1.简介

### 1.1 ajax 

ajax(asynchronous javascript and xml)，即异步 JS 和 XML

通过 ajax 可以在浏览器中向服务器发送异步请求，最大的优势 –> 无刷新获取数据

ajax 是一种将现有标准组合在一起使用的新方式


### 1.2 xml

xml 可扩展标记语言

被设计用来传输和存储数据

与 html 类似，不同的是 html 中都是预定义标签，而 xml 没有预定于标签，全部是自定义标签，用了表示一些数据

### 1.3 json

现在 xml 已经的传输和存储数据的功能已经被 json 取代了



### 1.4 ajax 的特点

#### 1.4.1 优点

1. 可以无需刷新页面与服务端进行通信
2. 允许根据用户事件来更新部分页面内容

#### 1.4.2 缺点

1. 没有浏览历史，不能回退
2. 存在跨域问题（同源）
3. seo（爬虫） 不友好



## 2. http 报文

### 2.1 http

hypertext transport protocol 超文本传输协议，详细规定了浏览器和万维网服务器之间互相通信的规则



### 2.2 请求报文

> 重点：格式、参数

+ 行   : POST    /s?ie=utf-8   HTTP/1.1

+ 头   : Host: …

  ​         Cookie: name=…

  ​         Content-type:application/x-www-form-urlencoded

  ​         User-Agent:chrome 83

+ 空行 :

+ 体    : username=admin&password=admin



### 2.3 响应报文

+ 行   : HTTP/1.1  200  OK

+ 头   : Content-type: text/html;charset=utf-8

  ​         Content-length:2048

  ​         Content-encoding:gzip

+ 空行 :

+ 体    : `<html>....</html>`



### 2.4 network 上的请求响应

+ Headers：请求内容
  + General
  + Response Headers：响应头和行
  + Requset Headers：请求头和行
  + Query String Parameters：请求体
    + get ： 查询字符串的键值对
    + post：查询字符串
+ Preview：预览响应过来的html
+ Response：响应的内容



## 3. ajax的使用

### 3.1 node express 的使用

使用node express搭建服务器

~~~js
import express from 'express';

const app = express();

app.get('/', (request, response) => {
  response.send('hello express');
})

app.listen(8000, () => {
  console.log("8000端口监听...");
})
~~~



### 3.2 原生ajax使用

1. 创建对象 
   
    ~~~js
    const xhr = new XMLHttpRequest();
    ~~~
    
2. 初始化 设置请求方法和url 
   
    ~~~js
    xhr.open('GET','http://127.0.0.1:8000/server');
    ~~~
    
3. 发送 设置参数
   
    ~~~js
    xhr.send();
    xhr.send('a=10&b=20');
    ~~~
    
4. 事件绑定 处理服务端返回的结果
   
    ~~~js
    xhr.onreadystatechange = function(){...}
    ~~~
    
    > 其中 readystate 是 xhr 对象的属性，
    > 其值为 0:初始状态 1:open后  2:send后  3:返回部分数据 4:返回全部数据
    
    > 其中 xhr的对象 
    > state:状态码 statusText:状态字符串 getAllResponseHeader():所有响应头 response:响应体





* 服务器端要做的操作：解决跨域问题
  
    ~~~js
    response.setHeader('Access-Control-Allow-Origin','*');
    ~~~
    
    
    
* if 设置请求头 :  在send前
  
     ~~~js
      xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded')
      xhr.setRequestHeader('name','lee')
     ~~~
  
  则 要在服务器端 设置允许的响应头：
  
  ~~~js
   response.setHeader('Access-Control-Allow-Headers','*');
  ~~~
  
  
  
* if 处理后端传的字符串json数据

    + 服务器

      ~~~js
       const data = { name : 'lee' }
       let str = JSON.stringify(data);
       response.send(str)
      ~~~

    + 前端处理
        手动转换数据格式：

      ~~~js
       let data = JSON.parse(xhr.response);
       res.value = data.name;
      ~~~

        自动转换：

      ~~~js
       //在open前 设置响应体数据的类型
       xhr.responseType = 'json'
       //在事件里直接使用就可以了
       res.value = xhr.response.name;
      ~~~

      

* 解决ie缓存问题：在open中加时间戳 保证每次发生的请求都不一样
    ~~~js
      xhr.open('GET','http://127.0.0.1:8000/ie?t=' + Date.now());
    ~~~
      
      
      
* 处理请求异常
   服务器可以设置一个定时器，三秒后才返回响应
   在open前：
   
   ~~~js
    //超时设置 
    xhr.timeout = 2000;
    //超时回调 
    xhr.ontimeout = function(){ alert('网络异常，请稍后重试') }
    //网络异常回调 可以在f12 使用offline 测试
    xhr.onerror = function(){ alert('你的网络似乎出现了问题') }
   ~~~
   
   
   
* 取消发送请求事件
  
    ~~~js
    let x = null;
    //发送请求 x 为 前面的xhr对象
    //使用 x.abort() 可以取消发送请求
    //利用这个方法可以处理 多次发送请求 只保留一个请求的情况
    ~~~
    
    

#### 3.2.1 原生ajax案例 – 点击按钮 将响应的数据放在文本框里




### 3.3 jquery 中的 ajax

1. get/post请求
     `$.get/post(url, data请求携带的参数, callback成功时的回调函数, type返回数据格式)  后面三个参数可选填`
     
     ~~~js
     $.get('http://127.0.0.1:8000/jq-server', {a:10,b:20}, function(data){
           log(data);
     },'json')
     ~~~
     
     
     
1. ajax通用请求
   
     ~~~js
     $.ajax({
          url:'http://127.0.0.1:8000/delay',   //url
          data: {a:10,b:20},       //参数
          type: 'GET',             //请求类型
          dataType:'json',         //响应体结果
          success:function(data){  //成功回调
               log(data);
          },
          timeout:2000,            //超时回调
          error:function(){...}    //失败回调
         headers:{...}            //自定义头部
    })
    ~~~
    
    

### 3.4 axios

1. 导入axios加载文件 bootcdn

2. axios可以配置 baseURL
   
    ~~~js
    axios.defaults.baseURL = 'http://127.0.0.1:8000';
    ~~~
    
3. axios使用 返回promise对象
    get请求: axios.get(url [, config] )
    post请求: axios.post(url [, data [, config]] ) //第二个是请求体参数
    axios请求: axios(config)
    
    ~~~js
     axios.get('/axios-server',{
           params:{          //url参数
                id:100,
                vip:7
           },
           headers:{         //请求头信息
                name:'lee',
                age:'20'
           }
    }).then(value => { ... } )
    ~~~
    
    ~~~js
     axios.post('/axios-server',{ username:'admin', password:'admin' },{
           params:{          //url参数
                id:100,
                vip:7
           },
           headers:{         //请求头信息
                name:'lee',
                age:'20'
           }
    }).then(value => { ... } )
    ~~~
    
    ~~~js
    axios({
        method:'POST',              //请求方法
        url:'/axios-server',        //url
        params:{id:100, vip:7 },    //url参数
        headers:{ a=10, b=20 },     //头信息
        data:{ username:'admin', password:'admin' },  //请求体参数
    }).then( res => { log(res) } )
    ~~~
    
    
### 3.5 fetch

~~~js
fetch('http://127.0.0.1:8000/fecth-server?vip=10',{
    method:'POST',                          //请求方法
    headers:{ name:'lee' },                 //请求头
    body:'username=admin&password=admin'    //请求体
}).then(res => {
    return res.text();  //字符串
    return res.json();  //json
}).then(res => {
    console.log(res)
})
~~~



## 4. 跨域问题

### 4.1同源策略

浏览器的一种安全策略

同源：协议、域名、端口号完全相同

违背同源策略就是跨域



### 4.2 同源的一个例子

html页面跟 发送请求时同一个地址

~~~js
//server.js
...
app.get('/home',(req,res) => {
    res.sendFile(__dirname + '/index.html');  //响应一个页面
})

app.get('/data',(req,res) => {
    res.send('用户数据');   //请求数据
})
~~~

~~~js
//index.html
...
btn.onlick = function(){
    const xhr = new XMLHttpRequest();
    // 因为是同源的 所以url 可以简写
    xhr.open('GET','/data');
    xhr.send();
    xhr.onreadystatechange = function(){
        ...
    }
}
~~~



### 4.3 jsonp 原理

主要就是利用了 `script` 标签的`src`没有跨域限制来完成的

缺点：

- 只能进行`GET`请求

优点：

- 兼容性好，在一些古老的浏览器中都可以运行

~~~js
//server.js
app.all('/jsonp', (req, res) => {
  // res.send('console.log(hello jsonp)')
  const data = {
    name: 'lee'
  }

  res.end(`handle(${JSON.stringify(data)})`);

})
~~~

~~~html
//html
<script>
    function handle(data) {
      const res = document.querySelector('div');
      res.innerHTML = data.name;
    }
</script>
<script src="http://127.0.0.1:8000/jsonp"></script>
~~~



### 4.4 jsonp 实践

~~~js
//server
app.all('/jsonp-exe', (req, res) => {
  // res.send('console.log(hello jsonp)')
  const data = {
    exist: 1,
    mes: '用户名存在'
  }

  res.end(`handle(${JSON.stringify(data)})`);

})
~~~

~~~html
<script>
  const p = document.querySelector('p')
  const input = document.querySelector('input')
  function handle(data) {
    input.style.border = '1px solid red';
    p.innerHTML = data.mes;
  }
  input.onblur = function () {
    // 获取用户的输入
    let username = this.value;
    // 向服务器发送请求 检测用户是否存在
    // 1.创建script
    const script = document.createElement('script');
    // 2.设置标签 src
    script.src = 'http://127.0.0.1:8000/jsonp-exe';
    // 3.插入到dom
    document.body.appendChild(script);
  }
</script>
~~~



### 4.5 jq-jsonp

### 4.6 cors

<img src="https://s1.328888.xyz/2022/04/09/X3BLX.jpg">

~~~js
//server
app.all('/cors', (req, res) => {
  // 1.设置响应头，允许跨域
  res.setHeader('Access-Control-Allow-Origin', '*');
  // 2.设置响应头
  res.setHeader('Access-Control-Allow-Headers', '*');
  res.setHeader('Access-Control-Allow-Methods', '*');
  // 3.响应体
  res.send('hello cors');

})
~~~

~~~html
<script>
  const btn = document.querySelector('button')
  const text = document.querySelector('textarea')

  btn.onclick = function () {
    const xhr = new XMLHttpRequest();
    xhr.open('GET', 'http://127.0.0.1:8000/cors');
    xhr.send();
    xhr.onreadystatechange = function () {
      if (xhr.readyState === 4) {
        text.value = xhr.response
      }
    }
  }
</script>
~~~

