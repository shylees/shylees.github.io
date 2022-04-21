---
title: 路径别名配置
date: 2021/4/21
updated: 2021/4/21
comments:
tags:
 - vue
categories:
 - learningNotes
layout:
permalink:
meta: true
---

[toc]

 在webpack.base.conf.js 里

 ~~~js
 resolve:{
     extensions:['.js','.vue','.json'],
     alias:{
         '@':resolve('src'),
         'assets':resolve('src/assets'),
         'components':resolve('src/components'),
         'views':resolve('src/views'),
     }
 }
 ~~~

 使用时：

 ~~~vue
 <template>
 	<img solt='item-icon' src='~assets/img/tabbar/home.svg'>
 </template>
 ~~~
 

