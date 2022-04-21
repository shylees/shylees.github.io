---
title: 03 jQuery事件
date: 2020/9/22
updated: 2020/9/26
comments:
tags:
 - jQuery
categories:
 - learningNotes
layout:
permalink:
meta: true
---


#### 1.jQuery事件注册

##### 单个注册事件

**语法：**

~~~javascript
element.事件(function(){})

$("div").click(function(){ 事件处理程序 })
~~~

其他事件跟原生基本一致

比如：mouseover、mouseout、blur、focus、change、keydown、keyup、resize、scroll等

~~~javascript
$(function(){
    //1.单个事件注册
    $("div").click(function(){
        $(this).css("background","purple");
    });
    $("div").mouseenter(function(){
        $(this).css("background","skyblue");
    });
})
~~~

#### 2.jQuery事件处理

##### 2.1事件处理on()绑定事件

on() 方法在匹配元素上绑定一个或多个事件处理函数

**语法：**

~~~javascript
element.on(events,[selector],fn)
~~~

* events：一个或多个用空格分隔的事件类型，如“click”或“keydown”

* selector：元素的子元素选择器

* fn：回调函数 即绑定在元素身上的监听函数

  <img src="https://s1.328888.xyz/2022/04/09/XkpPi.png" style="zoom:67%;" align="left" />

  <img src="https://s1.328888.xyz/2022/04/09/XksAv.png" style="zoom:67%;" align="left" />

  <img src="https://s1.328888.xyz/2022/04/09/Xk7Y0.png" style="zoom:67%;" align="left" />

~~~javascript
//2.事件处理on
//优势1 on可以绑定1个或多个事件处理程序
$("div").on({
    mouseenter:function(){
        $(this).css("background","skyblue");
    },
    click:function(){
        $(this).css("background","purple");
    },
    mouseleave:function(){
        $(this).css("background","blue");
    }
})
//优势1 if事件处理程序相同
$("div").on("mouseenter mouseleave",function(){
    $(this).toggleClass("current");
})

//优势2 on可以实现事件委托
$("ul").on("click","li",function(){
    alert(11);
})
//click 是绑在ul身上的 但是触发的对象是 ul 里面的小li

//优势3 on可以给未来动态规划创建元素绑定事件
$("ol li").click(function(){
    alert(11);  //因为还没有创建 所以没有办法实现
})

$("ol").on("click","li",function(){
    alert(11); //可以弹出弹窗
})

var li = $("<li>这是现在才加入的li<li>");
$("ol").sppend(li);

~~~

##### 2.2事件处理off()解绑事件

off()方法可以移除通过on()方法添加的事件处理程序

~~~javascript
$("p").off() //解绑p元素的所有事件处理程序

$("p").off("click") //解绑p元素上面的点击事件 后面的fn是监听函数名

$("ul").off("click","li") //解绑事件委托
~~~

<font color="red"> 如果有的事件只想触发一次，可以使用one()来绑定事件</font>

~~~javascript
//1.事件解绑 off
$("ul").off("click"); //解绑ul元素上面的点击事件
$("ul").off("click","li") ;
//2.one()但是它只能触发事件一次
$("p").one("click",function(){
    alert(11);
})
~~~

##### 2.3自动触发事件trigger()

~~~javascript
//1.元素.事件()
$("div").click(); //会触发元素的默认行为

//2.元素 .trigger("事件")
$("div").trigger("focus"); //会触发元素的默认行为
$("input").trigger("focus"); //其默认行为 光标闪烁

//3.元素.triggerHandler("事件") 就是不会触发元素默认行为
$("div").triggerHandler("click");
$("input").on("focus",function(){
    $(this).val("你好");
})
~~~

#### 3.jQuery事件对象

事件被触发，就会有事件对象的产生
`element.on(events,[selector],function(event){})`
阻止默认行为：`event.preventDefault() / return false`
阻止冒泡: `event.stopPropagation()`

~~~javascript
$(document).on("click",function(){
    console.log("点击了document");
})
$("div").on("click",function(event){
    console.log("点击了div");
    event.stopPropagation();  //if没有这个代码，则这个div点击后 会冒泡 上面的document也会打印出来
})
~~~



























































