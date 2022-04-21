---
title: MVC MVP MVVM
date: 2021/12/6
updated: 2021/12/6
comments:
tags:
 - vue
categories:
 - furtherNotes
layout:
permalink:
meta: true
---

>
> 整理 MV 系列框架概念
>
> http://c.biancheng.net/view/7743.html


## 1. MVC 框架

MVC 框架流程图 和 框架图 如下（实线表示调用，虚线表示通知）在 Controller 控制层会接收用户的所有操作，并根据写好的代码进行相应的操作



<img src="http://c.biancheng.net/uploads/allimg/200525/1-2005251033103O.gif" style="zoom:67%;float:left" ><img src='http://c.biancheng.net/uploads/allimg/200525/1-200525102U9463.gif' style="zoom:67%;float:left;" >

















<font color='red'>特点</font>：controller 控制 model 层将数据赋值给 view 层。

<font color='red'>缺点</font>：MVC 框架的大部分 逻辑和代码量 都集中在 controller 层，这带给 controller 层造成很大压力，且已经有独立处理事件能力的 view 层没有用到；controller 和 view 之间是一一对应的，断绝了 view 层复用的可能，因此产生了很多冗余的代码

<font color='red'>注</font>：controller 触发 view 时，并不会更新 view 层中的数据，

​         view 中的数据是通过监听 model 数据变化而自动更新的，与 controller 无关



## 2. MVP 框架

MVP — Model View Presenter

MVP 框架流程图 和 框架图 如下，

<img src="http://c.biancheng.net/uploads/allimg/200525/1-20052510440U32.gif" style="zoom:67%;float:left;" ><img src="http://c.biancheng.net/uploads/allimg/200525/1-200525103Z52a.gif" style="zoom:67%;float:left;" >



















在 MVC 中，view 可以通过访问 Model 来更新，但在 MVP 中，View 不能直接访问 Model ，必须通过 Presenter 提供的接口，然后 Presenter 再去访问 Model。

<font color='red'>特点</font>：Model 和 View 都必须通过 Presenter 来传递信息，所以完全分离了 View 和 Model ，双方不知道彼此的存在；因为 View 和 Model 没有关系，所以 View 可以抽离出来做成组件，在复用上比较好。

<font color='red'>缺点</font>：因为 View 和 Model 都需要经过 Presenter，致使 Presenter 比较复杂，维护起来会有一定问题；而且因为没有绑定数据，所有数据都需要 Presenter 进行 “手动同步”，代码量比较大，也会有比较多的冗余。

为了让 View 和 Model 的数据始终保持一致，避免同步，推出了 MVVM 框架：

## 3. MVVM 框架

MVVM 框架流程图 和 框架图 如下：

<img src="http://c.biancheng.net/uploads/allimg/200525/1-200525105GH58.gif" style="zoom:67%;float:left;" ><img src='http://c.biancheng.net/uploads/allimg/200525/1-200525105346422.gif' style="zoom:67%;float:left"  >















VM：ViewModel 把 Model 和 View 的数据同步自动化了，解决了 MVP 中数据同步比较麻烦的问题，不仅减轻了 ViewModel 的压力，同时使得数据处理更加方便 — 只需告诉 View 展示的数据是 Model 中的哪部分即可。

<font color='red'>特点</font>：ViewModel 双向绑定了 View 和 Model，因此，随着 View 的数据变化，系统会自动修改 MOdel 的数据，反之同理。

> 而 Presenter 是采用手动写方法来调用或者修改 View 和 Model。

<font color='red'>双向数据绑定</font>：双向数据绑定是一个模板引擎，它会根据数据的变化实时渲染，如图，View 和 Model 之间的修改都会同步到对方。

<img src='http://c.biancheng.net/uploads/allimg/200525/1-200525110113316.jpg' style="zoom:67%;" >

MVVM 中数据绑定方法一般有以下3种：

+ 数据劫持
+ 发布 - 订阅模式
+ 脏值检查

> 其中 Vue.js 用的就是发布 - 订阅模式，**Observer（数据监听器）**用户监听数据变化，如果数据变化，不论是在 View 还是 Model，Observer 都会知道，然后告诉 **Watcher（订阅者）**。**Compiler（指定解析器）**的作用是对数据进行解析，之后绑定指定的事件，在这主要用于更新视图。

Vue.js 数据绑定的流程：首先将需要绑定的数据用数据劫持方法找出来，之后用 Observer 监听这堆数据，如果数据发生变化，Observer 就会告诉 Watcher，然后 Watcher 会决定让哪个 Compiler 去做相应的操作，这就完成了数据的双向绑定。

 

## 4. 简述 3 种 框架 及其区别

1. MVC：model 层存放数据逻辑，view 层用于显示数据，controller 层用于将 model 层的数据赋值给 view 层，而在 model 层数据变化的时候， view 在监听到后再修改显示的数据，即 view 是可以访问到 model 的

   > 其实 所有处理业务的逻辑都在 controller 层上了，controller 压力繁重，而且 controller 层 跟 view 层其实算是一一对应的关系，所以代码难以复用

2. MVP：此时 model 层和 view 层是不知道彼此的存在的，当 model 数据变化后，需要经过 presenter 层反馈给 view 层，然后才进行改变，

   > 因为没有数据绑定，所有数据都要经过 presenter 手动同步，使得 presenter 代码繁重

3. MVVM：viewModel 层实现了model 和 view 的双向数据绑定，当一方数据改变的时候，就会经过 viewModel 将另一方的数据同步更改。

   > 发布者订阅模式：将要绑定的数据用数据劫持的方法找出来，用 数据监听器 observer 监听数据变化，当数据变化时，告诉 订阅者 Watcher，订阅者 找到适合的 指定解析器 去完成相应操作，完成了数据绑定
