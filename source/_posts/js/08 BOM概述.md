---
title: js bom对象概述
date: 2020/5/22
updated: 2020/5/22
comments:
tags:
 - js
categories:
 - learningNotes
layout:
permalink:
meta: true
---



#### 1.什么是BOM

BOM（Browser Object Model）浏览器对象模型，它提供了独立于内容而与浏览器窗口进行交互的对象，其核心对象是 window

BOM 由一系列相关的对象构成，并且每个对象都提供了很多方法和属性。



#### 2. 与DOM相比

+ DOM
  + 文档对象模型
  + DOM 就是把文档当作一个对象来看待
  + DOM 的顶级对象就是 document
  + DOM 主要学习的是操作页面元素
  + DOM 是 W3C 标准规范
+ BOM
  + 浏览器对象模型
  + 把浏览器当作一个对象看待
  + BOM 的顶级对象是  window
  + DOM 学习的是浏览器窗口交互的一些对象
  + BOM 是浏览器厂商再各自浏览器上定义的，兼容性差

#### 3.BOM 的构成

+ document
+ location
+ navigation 
+ screen
+ history

window 对象是浏览器的顶级对象，具有：

1. 是js访问浏览器窗口的一个接口
2. 是一个全局对象，定义在全局作用域中的变量、函数都会变成 window对象的属性和方法

在调用时可以省略window