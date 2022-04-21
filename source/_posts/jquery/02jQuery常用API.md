---
title: 02 jQuery常用API
date: 2020/9/20
updated: 2020/10/28
comments:
tags:
 - jQuery
categories:
 - learningNotes
layout:
permalink:
meta: true
---


####  1 jQuery选择器

##### 1.1jQuery基础选择器

<img src="https://s1.328888.xyz/2022/04/09/XkYXF.png" style="zoom:80%;" align="left" />

##### 1.2层级选择器

<img src="https://s1.328888.xyz/2022/04/09/XkTpW.png" style="zoom:60%;" align="left"  />

##### 1.3.1 知识铺垫—jQuery设置样式

~~~javascript
 $("div").css('属性','值')
~~~

~~~html
<div>div1</div>
<div>div2</div>
<div>div3</div>
<script>
    //1.获取四个div文件
    console.log($("div"));
    
    //2.给四个div文件设置背景  jQuery对象不能应style
    $("div").css("background","pink");
    //隐式迭代 就是把所有匹配的所有元素内部进行遍历循环 给每一个元素都添加css这个方法
</script>
~~~

##### 1.3.2 隐式迭代

遍历内部 DOM 元素，以为数组形式存储的过程就叫做 隐式迭代。
即：给匹配到的所有元素进行循环遍历，执行相应的方法

##### 1.4 jQuery 筛选选择器

<img src="https://s1.328888.xyz/2022/04/09/XkcFy.png" style="zoom:55%;" align="left" />

```html
<!-- 二级导航 -->
<ul class="nav"> 
    <li>   <!-- 一级导航 -->
        <ul> <!-- 二级导航-->
        </ul> 
    </li> 
    <!-- ...-->
</ul>

<script>
    $(function(){  //jQuery入口函数
        //鼠标经过
        $(".nav>li").mouseover(function(){
            //$(this) jQuery 当前元素 this 不加引号
            //show()显示元素 hide()隐藏元素
            $(this).children("ul").show();
        })
        //鼠标离开
        $(".nav>li").mouseout(function(){
            $(this).children("ul").hide();
        })
    })
</script>
```



##### 1.5jQuery 筛选方法

<img src="https://s1.328888.xyz/2022/04/09/Xk3ld.png" style="zoom:67%;" align="left" />

**注:** **1. **兄弟元素siblings 除了自身元素之外的所有亲兄弟

~~~javascript
$("ol .item").siblings("li").css("color","red");
~~~

  **2.**第n个元素

~~~javascript
var index=2;
//1.选择器
$("ul li:eq(2)").css("color","blue");
$("ul li:eq("+index+")").css("color","blue");
//2.选择方法
$.("ul li").eq(2).css("color","blue");
$("ul li").eq(index).css("color","blue")v;
~~~

**3.**判断是否有某个类名

~~~javascript
console.log($("div:first").hasClass("current"));
~~~

**4.**父子兄关系的获取

~~~javascript
//1.父 parent() 返回 最近一级的父级元素 亲爸爸
console.log($(".son").parent());
//2.子 亲儿子 children() 子代选择器 ul>li 
$(".nav").children("p").css("color","red");
//     所有孩子 find() 后代选择器
$(".nav").find("p").css("color","red");
~~~

**5.**返回指定数组的祖先元素

~~~html
<div class="one">
    <div class="two">
        <div class="three">
            <div class="four"></div>
        </div>
    </div>
</div>
<script>
    console.log($(".four").parent().parent().parent());
    console.log($(".four").parents()); //伪数组的形式
    console.log($(".four").parents(".one"));  //指定.one
</script>
~~~



##### 1.6jQuery 的排他思想

利用兄弟关系

~~~javascript
//1.隐式迭代 给所有的按钮的绑定了点击事件
$("button").click(function(){
    //2.当前背景元素发生变化
    $(this).css("background","pink");
    //3.其他兄弟去掉背景颜色
    $(this).siblings("button").css("background","");
})
~~~

<font size=3 color="red">jQuery得到当前元素的索引号   $(this).index</font>

#### 2.jQuery的样式操作

##### 2.1 操作css方法

~~~javascript
$("div").css("width","300px");
$("div").css("width",300);
//属性名 一定得加引号 否则会被认为是变量

$("div").css({
    width:400,
    height:400px;
    backgroundColor:"red";
})
~~~

##### 2.2设置类样式方法

~~~javascript
$(function(){
    //1.添加类 addClass
    $("div").click(function(){
        $(this).addClass("current");
    });
    //2.删除类 removeClass
    $("div").click(function(){
        $(this).removeClass("current");
    });
     //3.切换类 toggleClass
    $("div").click(function(){
        $(this).toggleClass("current");
    });
})
~~~

~~~javascript
//实例 点击上面的li 当前的li 添加current 类 其余兄弟移除该类
$(".tab_list li").click(function(){
    //链式编程操作
    $(this).addClass("current").siblings().removeClass("current");
    //2.点击的同时 得到当前li的索引号
    var index=$(this).index();
    console.log(index);
    //3.让下部里面的索引号的item显示，其余的让item隐藏
    $(".tab_con .item").eq(index).show().siblings().hide();
});
~~~

##### 2.3类操作与className区别

原生js中className会覆盖元素原来里面的类名

jQuery里面类操作只是对指定类进行操作，不影响原先的类名

~~~html
<div class="one"></div>
<script>
    var one=document.querySelector(".one");
    one.className="two";  //这里div的类名只有two
    
    $(".one").addClass("two"); //这里div的类名 有one two
    
    $(".one").removeClass("one"); //这里div的类名 只有two(跟上代码一起)
</script>
~~~

#### 3.jQuery效果

<img src="https://s1.328888.xyz/2022/04/09/XkOP3.png" style="zoom:60%;" align="left" />

##### 3.1显示隐藏效果

* 显示语法规范

  ~~~javascript
  show([speed],[easing],[fn])  //fn 回调函数 前面做完了才会 实现的函数
  ~~~

* 隐藏语法效果

  ~~~javascript
  hide([speed],[easing],[fn])
  ~~~

* 显示隐藏切换语法效果

  ~~~javascript
  toggle([speed],[easing],[fn])
  ~~~

  

* 显示/隐藏/切换参数


~~~javascript
$(function(){
    $("button").eq(0).click(function(){
        $("div").show(1000,function(){
            alert(1);  //完成前面操作之后 弹出窗口
        });
    })
     $("button").eq(0).click(function(){
        $("div").hide(1000,function(){
            alert(1);  //完成前面操作之后 弹出窗口
        });
    })
     $("button").eq(0).click(function(){
        $("div").toggle();  //一般情况下 我们不加 参数 直接显示隐藏就ok
    })
})
~~~

##### 3.2滑动效果

* 上滑效果语法规范

  ~~~javascript
  slideDown([speed],[easing],[fn])
  ~~~

* 下滑效果语法规范

  ~~~javascript
  slideUp([speed],[easing],[fn])
  ~~~

* 滑动切换效果语法规范

  ~~~javascript
  slideToggle([speed],[easing],[fn])
  ~~~

* 上滑/下滑/切换效果参数

##### 3.3事件切换

* 事件切换语法规范

  ~~~javascript
  hover([over],out)
  ~~~

​       over : 鼠标移到元素上要触发的函数（相当于mouseenter）

​       out ：鼠标移出元素要触发的函数（相当于mouseleave）

​       hover就是鼠标经过和离开的复合写法  

 ~~~javascript
  $(".nav>li").hover(function(){
      $(this).children("ul").slideDown(200);
  },function(){
      $(this).children("ul").slideUp(200);
  });
 ~~~

   <font size=3 color="red">事件切换 hover 如果只写一个函数 那么鼠标经过 离开都会触发这个函数</font>

~~~javascript
$(".nav>li").hover(function(){
    $(this).children("ul").sildeToggle();
})
~~~

##### 3.4动画队列及其停止排队方法

1. **动画或效果队列：**动画或者效果一旦触发就会执行，如果多次触发，就造成多个动画或效果排队执行

2. **停止排队：**stop()

   ​          stop()方法用于停止动画或效果

   ​          注：stop()写到动画或者效果的<font size=3 color="red">前面，相当于停止结束上一次的动画</font>

   ~~~javascript
   $(".nav>li").hover(function(){
       $(this).children("ul").stop().sildeToggle();
   })
   ~~~

##### 3.5淡入淡出效果

* 淡入语法规范

  ~~~javascript
  fadeIn([speed],[easing],[fn])
  ~~~

* 淡出语法规范

  ~~~javascript
  fadeOut([speed],[easing],[fn])
  ~~~

* 淡入淡出切换语法规范

  ~~~javascript
  fadeToggle([speed],[easing],[fn])
  ~~~

* 淡入淡出切换参数

* 渐进方式调整到指定的不透明度

  ~~~javascript
  fadeTo([speed],[opacity],[easing],[fn])
  ~~~

* 参数

~~~javascript
$(function(){
    $("button").eq(0).click(function(){
        //淡入
        $("div").fadeIn(1000);
    });
    $("button").eq(1).click(function(){
        //淡入
        $("div").fadeOut(1000);
    });
    $("button").eq(2).click(function(){
        //淡入
        $("div").fadeToggle(1000);
    });
    $("button").eq(3).click(function(){
        //淡入
        $("div").fadeTo(1000,0.5);
    });
})
~~~

~~~javascript
//突出显示 案例
$(function(){
    //鼠标进入的时候，其他的li标签头透明度0.5
    $(".wrap li").hover(function(){
        $(this).siblings().stop().fadeTo(400,0.5);
    },function(){
        //鼠标离开 其他li 透明度改为 1
        $(this).siblings().stop().fadeTo(400.1);
    })
})
~~~

##### 3.6自定义动画 animate

* 语法

  ~~~javascript
  animate(params,[speed],[easing],[fn])
  ~~~

* 参数

~~~javascript
$("button").click(function(){
    $("div").animate({
        left:500,
        top:300
    },500);
})
~~~


#### 4.jQuery属性操作

##### 4.1设置或获取元素固有属性值 prop()

* 获取属性语法：

  `prop("属性")`

* 设置属性语法：

  `prop("属性","属性值")`

~~~html
<a href="" title="都挺好">都挺好</a>
<input type="checkbox" name="" id="" checked>
<script>
    $(function(){
        //element.prop("属性名") 获取属性值
        console.log($("a").prop("href"));
        $("a").prop("title","我们都挺好");
        $("input").change(function(){
            console.log($(this).prop("checked"));
        })
    })
</script>
       
~~~

##### 4.2设置或获取元素自定义属性值attr()

* 获取属性：

  `attr("属性") //类似元素getAttribute()`

* 设置属性：

  `attr("属性","属性值") //类似原生setAttribute()`

~~~javascript
console.log($("div"),attr("index"));
~~~

##### 4.3数据缓存data()

data()方法可以在指定元素上存取数据，并不会修改DOM元素结构，一旦页面刷新，之前存放的数据都将被移除

* 附加数据语法：

  `data("name","value) //向被选择元素附加数据` 

* 获取数据语法：

  `data('name') //向被选元素获取数据`

  <font size=3 color="red">可以读取html5自定义属性data-index，得到数字型</font>


#### 5.jQuery内容文本值

主要针对元素的内容还有表单的值的操作

* 普通元素内容html() 相当于原生inner HTML

  `html() //获取元素的内容`

  `html("内容") //设置元素的内容`

* 普通元素文本内容text() 相当于原生innerText

  `text() //获取元素的文本内容`

  `text("内容") //设置元素的文本内容`

* 表单元素 val()

  `val() //获取表单的值`

  `val("内容") //设置表单内容`

~~~javascript
//1. 获取设置元素内容 html() 
console.log($("div").html()); //会打印出 标签
$("div").html("123") 000

//2. 获取设置元素文本内容 text() 
console.log($("div").text()); //打印出文本
$("div").text("123");

//3. 获取设置表单值 val()
console.log($("input").val());
$("input").val("123");
~~~

#### 6.jQuery元素操作（遍历、创建、添加、删除）

##### 6.1遍历元素

jQuery隐式迭代是对同一类元素做同样的操作，如果想要给同一类元素做不同操作，就需要用到遍历

**语法1：**

~~~javascript
$("div").each(function(index,domEle){xxx;})
~~~

* each()方法遍历匹配的每一个元素，主要用DOM处理

* 里面的回调函数有2个参数：index是每个元素的索引号；domEle是每个<font color="red">DOM对象，不是jQuery对象</font>

* <font color="red">所以想要使用jQuery方法，需要给这个dom元素转换为jQuery对象</font>

  ~~~javascript
  $(function(){
      var arr=["red","green","blue"];
      $("div").each(function(i,domEle){
          //回调函数第一个参数是索引号 可以自己指定索引号名称 
          console.log(i);
          //回调函数第二个参数是dom元素对象
          console.log(domEle);
          //domEle.css("color"); dom对象没有css方法
          $(domEle).css("color",arr[i]);
      })
  })
  ~~~

**语法2：**

~~~javascript
$.each(object,function(index,element){ xxx; })
~~~

* $.each()方法可用于遍历任何对象，主要用于<font color="red">数据处理，比如数组，对象</font>

* 里面的函数有2个参数：index是每个元素的索引号element遍历内容

  ~~~javascript
  $.each(arr,function(i,ele){
      console.log(i);
      console.log(ele);
  })
  
  $.each({
      name:"andy",
      age:18
  },function(i,ele){
      console.log(i);
      console.log(ele);    
  })
  ~~~

##### 6.2创建元素

**语法：**

~~~javascript
$("<li></li>"); //动态创建一个li
~~~

##### 6.3添加元素

1. 内部添加

   ~~~javascript
   element.append("内容") //把内容放入匹配元素内部  最后面 类似与appendChild
   
   element.prepend("内容") //把内容放入匹配元素内部 最前面
   ~~~

2. 外部添加

   ~~~javascript
   element.after("内容") //把内容放入目标元素后面
   
   element.before("内容") //把内容放入目标元素前面
   ~~~

   <font color="red">内部添加元素 生成之后 是父子关系</font>

   <font color="red">外部添加元素 生成之后 是兄弟关系</font>

##### 6.4删除元素

**语法：**

~~~javascript
element.remove() //删除匹配的元素（本身） 自杀

element.empty() //删除匹配元素集合中所有子节点 孩子

element.html("") //清空匹配的元素内容 孩子
~~~

~~~javascript
$(function(){
    //1.创建元素
    var li=$("<li>我是后来创建的li</li>");
    //2.添加元素
    
    //（1）.内部添加
	$("ul").append(li) //把内容放入匹配元素内部  最后面 类似与appendChild
	$("ul").prepend(li) //把内容放入匹配元素内部 最前面
    //（2）.外部添加
    var div=$("<div>我是后妈生的</div>");
    $(".test").after(div);
    $(".test").before(div);
    
    //3.删除元素
    $("ul").remove(); 
    $("ul").empty();
    $("ul").html("");
})
~~~

#### 7.jQuery尺寸、位置操作

##### 7.1jQuery尺寸

<img src="https://s1.328888.xyz/2022/04/09/XkFf2.png" style="zoom:60%;" align="left" />

~~~html
<style>
    div{
        width:200px;
        height:200px;
        background-color:pink;
        padding:10px;
        border:15px solid red;
        margin:20px;
    }
</style>
<script>
    $(function(){
        //1.width()/height() 获取设置元素 width和height大小
        console.log($("div").width()); //200
        //$("div").width(300);  //300
        
        //2.innerWidth() / innerHeight() 获取设置元素 width 和height ＋padding 大小
        console.log($("div").innerWidth()); //220
        
        //3.outerWidth() / outerHeight() 获取设置元素 width 和 height+padding +border 大小
        console.log($("div").outerWidth()); //250
        
        //4.outerWidth(ture) /outerHeight(true) 获取设置 width 和 height+padding+border+margin 大小
        console.log($("div").outerWidth(true)); //290
    })
</script>
~~~

##### 7.2jQuery位置 

位置主要有三个：offset ()、position()、scrollTop() / scrollLeft() 

<img src="https://s1.328888.xyz/2022/04/09/XkvGX.png" style="zoom:60%;" align="left" />

<img src="https://s1.328888.xyz/2022/04/09/XkMOC.png" style="zoom:89%;" align="left" />

<img src="https://s1.328888.xyz/2022/04/09/Xkoh1.png" style="zoom:50%;" align="left" />

~~~javascript
$(function(){
    //1.获取设置距离文档的位置(偏移) offset
    console.log($(".son").offset());
    console.log($(".son").offset().top);
    $(".son").offest({
        top:200,
        left:200
    });
    //2.获取距离大有定位父级位置（偏移）position 如果没有带有定位的父级，则以文档为准
    //这个方法只能获取 不能设置
    console.log($(".son").position());
    
    //3.被卷去的头部 scrollTop() /被卷去的左侧 scrollLeft()
    //页面滚动事件
     $(window).scroll(function() {
        console.log($(document).scrollTop());
    })
})
~~~

~~~html
<script>
</script>
~~~

















