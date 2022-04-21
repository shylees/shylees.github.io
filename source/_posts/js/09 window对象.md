---
title: js window对象常见事件
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



### 1. 窗口加载事件：onload、DOMContentLoaded

`onload`: 当文档内容完全加载完成时会触发该事件（包括图像、脚本、css、文件等），就调用的处理函数

`DOMContentLoaded`: 此事件触发时，仅当DOM加载完成，不包括样式表、图片、flash等等   ie9+

> 页面图片很多，从用户访问到onload触发可能需要较长的时间，交互效果就不能实现，此时用DOMContentLoaded比较合适 ，速度较快

触发方式：

+ 传统：`window.onload=function(){}`  只能写一次  多次 以最后一个为准
+ 监听：`window.addEventListener("load",function(){});` 没有次数限制	



### 2. 调整窗口大小事件: onresize

只要窗口发生像素变化就会触发这个事件|  利用此完成响应式布局



### 3. 定时器：setTimeout、setInterval

+ `window.setTimeout(调用函数，[延迟的毫秒数])`  : 该定时器再定时器到期后执行调用函数
  + window可以省略
  + 这个调用函数可以   直接写函数  或者  写函数名  或者  采取字符串'函数名()' 。第三种不推荐
  + 延迟的毫秒数省略默认是，单位是毫秒
  + 因为定时器可能有很多，所以经常给定时器赋值一个标识符
  + 这个函数需要等待时间，时间到了才去调用这个函数，因此称回调函数
+ `window.clearTimeout(timeout ID)`：停止setTimeout()定时器：
  + window可以省略   里面的参数就是定时器的标识符
+ `window.setInterval(回调函数，[间隔的毫秒数])`: 方法重复调用一个函数 ，每隔这个时间  就去调用一次回调函数
  + window可以省略	
  + 这个调用函数可以    直接写函数  或者  写函数名  或者  采取字符串'函数名()'  三种形式
  + 延迟的毫秒数省略默认是0，if写，必须是毫秒		
  + 因为定时器可能有很多，所以经常给定时器赋值一个标识符

+ `window.clearInterval(interval ID)`: 停止setInterval()定时器
  + window可以省略   里面的参数就是定时器的标识符







