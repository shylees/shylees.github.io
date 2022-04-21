---
title: js loaction、navigator、history对象概述
date: 2020/5/21
updated: 2020/5/21
comments:
tags:
 - js
categories:
 - learningNotes
layout:
permalink:
meta: true
---

### 1. location对象

#### 1.1 概述

用于获取或设置窗体的 url，并且可以用于解析url，应为返回的是一个对象，所以我们将这个属性也称为location对象。

#### 1.2 url 概述

统一资源定位器，是互联网上标准资源的地址。互联网上的每个文件都有一个唯一的url，它包含的信息指出文件的位置以及浏览器怎么处理它

一般语法为：`protocol：//host[ : port]/path/[?query]#fragment `

+ `http://www.itcast.c/index.html?name=andy&age=18#link`
+ 通信协议 — 常用http、ftp、maito
+ 主机（域名）— www.iteima.com
+ 端口号 —  可选，省略时使用方案的默认端口  如http的为80
+ 路径 — 由0或多个/隔开的字符串  一般用来表示主机上的一个目录或文件地址
+ 参数 — 以键值对的形式  通过&分隔开来
+ 片段 — #后面内容 常见于两节  锚点

#### 1.3 location 对象的属性

+ `location.href ` —  获取或设置整个url
+ `location.host` —  返回主机（域名）
+ `location.port` — 返回端口号   if未写返回  得空字符串
+ `location.pathname` — 返回路径
+ `location.search` — 返回参数
+ `location.hash` — 返回片段

#### 1.4 location 对象的方法

+ `location.assign()`    跟href一样 可以跳转页面（重定向页面） 能后退  
+ `location.replace()`  替换当前页面  不记录历史 不能后退	
+ `location.reload()`    重新加载页面 相当于刷新按钮或者f5 if参数为true 强之刷新  ctrl+5



### 2.navigator对象

包含有关浏览器的信息，它由很多属性，我们最常用的是userAgent，该属性可以返回由客户机发送服务器的use-agent头部的值

以下前端代码可以判断用户那个终端打开页面 实现跳转

~~~js
if((navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|Webos|Symbian|Windows Phone)/i))){
	window.location.href="(页面地址)";  //手机
}else{
	window.location.href=""; //电脑
}
~~~

### 3. history对象

与历史记录进行交互，该对象包含用户（在浏览器窗口中）访问过的url

+ back()      可以后退功能
+ forward() 前进功能
+ go(参数)   前进后退功能  参数是1  前进一个页面  如果是-1  后退1个页面