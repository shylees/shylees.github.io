---
title: 01 jQuery概述 
date: 2020/9/19
updated: 
comments:
tags:
 - jQuery
categories:
 - learningNotes
layout:
permalink:
meta: true
---



#### 1 jQurey 概述

##### 1.1 jQuery的概念

 jQuery 是一个快速简介的 js 库，其设计宗旨是 “write less，do more”；
 j：js
 Query：查询
 即是把js中的 dom 操作做了封装，使之可以快速查询使用里面的功能

 jquery 封装了js常用的功能代码，优化了 DOM 操作、事件处理、动画设计、ajax交互...
 学习jq的本质就是学习调用这些方法


##### 1.2 jQuery的优点

  + 轻量级
  + 跨浏览器兼容
  + 链式编程、隐式迭代
  + 对事件、样式、动画支持，大大简化了DOM操作
  + 支持插件扩展开发
  + 免费开源

  #### 2 jQuery的基本使用

  ##### 2.1 jQuery的下载 

   **官方网址**：https://jquery.com/
    (下方右使用说明 
    右边有棕色的“download”，左键点击
    production jQuery: 生产版本 压缩过 工作用的
    development jQuery: 开发版本 无压缩
    选其一 全选ctrl+A 复制 
    新建一文件 jquery.min.js  粘贴代码)

**版本的区别**
    + 1x: 兼容IE 678等低版本浏览器，官网不再更新
    + 2x: 不兼容IE 678等低版本浏览器，官网不再更新
    + 3x: 不兼容IE 678等低版本浏览器，官方主要更新维护版本


  ##### 2.2 jQuery的使用

   把jq引用到html页面中的<head>即可

   ```html
<script scr="jquery.min.js"></script>
   ```



**2.1 2.2用不了的话 用下面**

```java
//jquery-3.3.1：字节跳动jquery压缩版引用地址: （速度快推荐！）
<script src="https://s3.pstatp.com/cdn/expire-1-M/jquery/3.3.1/jquery.min.js"></script>

//jquery-3.2.1：字节跳动jquery压缩版引用地址: （速度快推荐！）
<script src="https://s3.pstatp.com/cdn/expire-1-M/jquery/3.2.1/jquery.min.js"></script>

//jquery-1.11.3：百度压缩版引用地址: 
<script src="https://libs.baidu.com/jquery/1.11.3/jquery.min.js"></script>

//jquery-1.5.2：百度压缩版引用地址:
<script src="https://libs.baidu.com/jquery/1.5.2/jquery.min.js"></script>
```



  ##### 2.3 jQuery的入口函数   

   ```javascript
 $(function(){
     ...  //此处是页面DOM加载完成的入口
 });  
   ```

 **或者**

   ```javascript
 $(document).ready(function(){
     ...  //此处是页面DOM加载完成的入口
 });
   ```

 <img src="img\2.3入口函数.png" style="zoom:80%;" />

   ```javascript
//使div元素隐藏
$(function(){
    $('div').hide();
})
   ```

##### 2.4 jQuery的顶级对象$

1. $是jQuery的别称 在代码中可以用jQuery代替\$
2. $是jQuery的顶级对象，相当于js中的window，把元素利用￥包装成jQuery对象，就可以调用看query的方法 

##### 2.5.1 jQuery对象和DOM对象

1. DOM对象：用原生js代码获取的对象

   `var myDiv=document.querySelector('div');`

2. jQuery对象：用jQuery方式获取过来的对象  本质：<font color="red">通过$把DOM元素进行包装</font>

   `$('div');`  //获取过来是伪数组的形式

3. <font color="red">jQuery对象只能用jQuery方法  DOM对象只能用原生的js的属性和方法</font>

   两者用`console.dir(myDiv)/console.dir($('div'))`打印出来的结果不一样

##### 2.5.2 jQuery对象和DOM对象的相互转换

jQuery对象与DOM对象之间是可以相互转换的，因为原生js比jQuery更大，原生的一些属性和方法jQuery没有封装，if要使用，jQuery对象就得转换为DOM对象

1. DOM对象转换为jQuery对象：<font color="red">$(DOM对象)</font>

   ```html
   <video src="mov.mp4"></video>
   <script>
       $('video'); //直接获取视频，得到jQuery对象
       var myvideo = document.querySelector('video'); //dom对象
       
       //1.dom-->jquery
       $(myvideo);
       //.play()方法 播放video js原生方法
       myvideo.play()  // √
       //$(myvideo).play() ×   
   </script>
   ```

2. jQuery对象转换为DOM对象：

   `$(‘div’)[index]  //index是索引号`

   `$(’div‘).get(index)  //index是索引号`

   ```javascript
   //往中间插入
   
   //2.jQuery对象转换为dom对象
   $('video')[0].play()  //√  因为$('video')[0]已经是dom对象
   $('video').get(0).play() //也可
   ```
