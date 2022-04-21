---
title: 01 Vue入门
date: 2021/3/9
comments:
tags:
 - vue
categories:
 - learningNotes
layout:
permalink:
---

## 1. 认识Vuejs

### 1.1 为什么学习Vuejs

### 1.2 简单认识Vuejs

1. Vue（读音类似view）

2. Vue是一个渐进式框架（作为一部分嵌入页面js）

   > Vue全家桶：Core+Vue-router+Vuex

3. Vue有很多特点和Web开发中常见的高级功能

   + 解耦视图和数据
   + 可复用的组件
   + 前端路由技术
   + 状态管理
   + 虚拟DOM

## 2. Vue.js安装方式

### 2.1 方式一：CDN引入

+ 选择引入开发环境版本还是生产环境版本

  ~~~html
  <!-- 开发环境版本，包含了由版主的命令行警告 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  
  <!-- 生产环境版本，优化了尺寸和速度 -->
  <script src="https://cdn.jsdelivr.net/npm/vue"></script>
  ~~~

### 2.2 方式二：下载和引入

开发环境：https://vuejs.org/js/vue.js

生产环境：https://vuejs.org/js/vue.min.js

### 2.3 方式三：NPM安装管理

+ 后续通过webpack和CLI的使用，我们使用该方式

## 3. Vuejs初体验

### 3.1 Hello Vuejs

+ vue：编程范式：声明式编程
+ js：编程范式：命令式编程

### 3.2 Vue列表展示

### 3.3 案例：计数器

语法糖：简写的另一种说法

## 4. Vuejs的MVVM

MVVM：（Model View View Model）最重要的是中间的ViewModel层，是View和Model之间安定桥梁

> 学习一个概念最好的方式是去看维基百科
>
> https://zh.wikipedia.org/wiki/MVVM

### 4.1 Vue中的MVVM

<img src='https://s1.328888.xyz/2022/04/09/XOxmt.jpg' style="zoom:60%;" >

<img src='https://s1.328888.xyz/2022/04/09/XaQoe.jpg' style="zoom:60%;" >

## 5. 创建Vue实例传入的options

+ options中可以包含的选项：https://cn.vuejs.org/v2/api/#%E9%80%89%E9%A1%B9-%E6%95%B0%E6%8D%AE

+ 目前掌握这些选项：

  1. el：

     + <font color='red'>类型：string|HTMLElement</font>
     + 作用：决定之后Vue实例会管理哪一个DOM

  2. data：

     + <font color='red'>类型：Object|Function （组件中data必须是一个函数）</font>
     + 作用：Vue实例对应的数据对象

  3. methods：

     + <font color='red'>类型：{ [key:string]:Function }</font>

     + 作用：定义属于Vue的一些方法，可以再其他地方调用，也可以在指令中使用

       > 不能使用箭头函数

### 5.1 方法跟函数的区别

方法：method

函数：function

一般在js中写的都是函数

但与实例挂钩的就是方法，例如类里面和Vue里面，方法是面向对象的

### 5.2 Vue的生命周期

事物从诞生到死亡

到GitHub下载源码时：不要直接下载开发版本，选择tag最新的稳定版

## 模板

缩进更多使用两个空格