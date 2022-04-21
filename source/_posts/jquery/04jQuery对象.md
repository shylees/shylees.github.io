---
title: 04 jQuery对象
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


#### 1.jQuery对象拷贝

如果想要把某个对象拷贝（合并）给另一个对象使用，此时可以使用$.extend()方法

**语法：**

~~~javascript
$.extend([deep]，target,object1,[objectN])
~~~

* deep：如果设为true 为深拷贝 ，默认false 浅拷贝
* target：要拷贝的目标对象
* object1：待拷贝到第一个对象的对象
* objectN：待拷贝到第N个对象的对象
* 浅拷贝是把被拷贝的对象<font color="red">复杂数据类型中的地址</font>拷贝给目标对象，<u>修改目标对象 会影响被拷贝对象</u>
* 深拷贝 前面＋true 完全克隆（拷贝的对象，而不是地址），<u>修改目标对象不会影响被拷贝对象</u>

~~~javascript
$(function(){
    var targetObj={
        id:0
    };
    var obj={
        id:1,
        name:"andy"，
        msg:{    //!!!
        age:18
    }
    };
    $.extend(targetObj,obj);
    console.log(targetObj); //会覆盖targetObj 里面原来的数据
	//1.浅拷贝是把被拷贝的对象复杂数据类型中的地址拷贝给目标对象
	targetObj.msg.age=20;
	console.log(targetObj);  //msg.age=20
	console.log(obj); //msg.age=20
	//2.深拷贝把里面的数据完全复制一份给目标对象 如果里面有不冲突的属性 会合并到一起
	$.extend(true,targetObj,obj);
	targetObj.msg.age=20;
	console.log(targetObj);  //msg.age=20
	console.log(obj); //msg.age=18
})
~~~

<img src="https://s1.328888.xyz/2022/04/09/Xkq6O.png" style="zoom:70%;" align="left" />

#### 2.多库共存

<img src="https://s1.328888.xyz/2022/04/09/XkzdS.png" style="zoom:60%;" align="left" />

~~~javascript
$(function(){
    function $(ele){   //把$用在这个函数上
        return document.querySelector(ele);
    }
    console.log($("div"));
    //1.如果$符号冲突 我们就用jQuery 代替$
    //$.each(); 这个不能用 因为$.each()不是一个函数 $被占用
    jQuery.each();
    //2.让jQuery 释放对$ 的控制权 让用自己命名的东西
    var suibian=jQuery.noConflict();
    console.log(suibian("span"));
    suibian.each();
})
~~~

#### 3.jQuery插件

<img src="https://s1.328888.xyz/2022/04/09/Xktbm.png" style="zoom:70%;" align="left" />

<img src="https://s1.328888.xyz/2022/04/09/XkWyA.png" style="zoom:70%;" align="left" />























