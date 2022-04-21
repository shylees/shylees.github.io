---
title: 节流throttle、防抖debounce 
date: 2021/11/14
updated: 2021/11/14
comments:
tags:
 - js
categories:
 - furtherNotes
layout:
permalink:
meta: true
---


## call、bind、apply、this 

### 1. this 指向

**<font color='red'>this 永远指向最后调用它的那个对象</font>**（执行时）

**匿名函数的 this 永远指向 window**

> 非严格模式 全局对象 window 
>
> 严格模式 全局对象是 undefined



### 2. 改变 this 指向

+ 使用箭头函数
+ 函数内部使用 `_this = this`
+ 使用 `apply`、`call`、`bind`
+ `new` 实例化对象



#### 2.1 箭头函数

**箭头函数的 this 始终指向函数定义时的 this，而非执行时。**

箭头函数中没有 this 绑定，必须通过查找作用域链来决定其值，如果箭头函数被非箭头函数包含，则 this 绑定的是最近一层非箭头函数的 this，否则 undefined。



#### 2.2 _this = this

相当于把 this 的值 保存在另一个变量 _this 



#### 2.3 <font color='red'>apply、call、bind</font>

##### 2.3.1 apply

apply() 方法调用一个函数，其具有一个指定的 this 值，以及 作为一个数组（类数组对象）通过的参数

`func.apply(thisArg, [argsArray])`

+ thisArg：在 func 函数运行时指定的 this 值。

  > 注：指定的 this 值并不一定是该函数执行时真正的 this 值，**如果函数处于非严格模式下，则指定为 null 或 undefined 会自动指向全局对象，值为原始值的 this 会指向该原始值的自动包装对象。**

+ argsArrays：一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 fun 函数。

  > 如果该参数的值为 null 或 undefined，则表示不需要传入任何参数。



##### 2.3.2 call

call 与 apply 基本类似，只有传入参数的不同

`func.call(thisArg[, arg1[, arg2...]])`

+ call 接收的 若干个参数列表     `b.call(a, 1, 2)`
+ apply 接收 一个包含多个参数的数组  `b.apply(a, [1,2])`



##### 2.3.3 bind

bind() 方法**创建一个新的函数**，所以需要被调用。

`b.bind(a, 1, 2)()`



#### 2.4 函数调用

方法种类：

1. 作为一个函数被调用

   > ~~~js
   > function a(){...}
   > a();
   > ~~~

2. 作为对象的方法被调用

   > ~~~js
   > let a = { fn : function(){ ... } };
   > a.fn();
   > ~~~

3. 使用构造函数调用函数

   > ~~~js
   > function func(arg){
   >     this.age = arg;
   > }
   > let a = new func(18)
   > a.age;
   > ~~~

4. 作为函数方法调用函数（ call、apply）

   > 在 js 中，函数是对象。
   >
   >  js 函数有自己的属性和方法，其中 call() 和 apply() 是预定义的函数方法，
   >
   > 其可用于调用函数，两个方法的第一个参数必须是对象本事。
   >
   > 在 严格模式，调用函数时 第一个参数会成为 this 的值，即使该参数不是一个对象
   >
   > 在非严格模式，如果第一参数是 null / undefined，将使用全局对象替代

   

   

   

   















