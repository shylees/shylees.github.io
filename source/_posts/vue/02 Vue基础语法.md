---
title: 02 Vue基础语法
date: 2021/3/9
updated: 2021/3/12
comments:
tags:
 - vue
categories:
 - learningNotes
layout:
permalink:
---


## 1.插值语法（p12-13）

### 1.1 Mustache语法

+ 插值表达式`{{ }}`
+ 在内容区书写的 不能在属性值书写

+ 不仅可以直接写变量 ，也可以写简单的表达式

### 1.2 其他指令的使用

#### v-once

+ 某些情况下，可能不希望界面随意的跟随改变
+ 该指令后面不需要跟任何表达式/值
+ 该指令表示元素和组件只渲染一次，不会随着数据的改变而改变

~~~html
<div id='app'>
    <h2 v-once>{{ message }}</h2>
</div>
~~~



#### v-html  

+ 某些情况下，从服务器请求到的数据本身就是html代码，if直接通过`{{}}`来输出，会将html代码也一起输出
+ 该指令会将string的html解析出来并且进行渲染
+ 该指令后面往往会跟上一个个string类型

~~~html
<div id='app'>
    <h2 v-html='url'></h2>
</div>
<script>
	const app = new Vue({
        el:'#app',
        data:{
            url:'<a href="https://www.baidu.com">百度</a>'
        }
    })
</script>
~~~



#### v-text

+ 该指令和Mustache比较相似：都是用于将数据显示在界面中
+ 该指令通常情况下，接收一个string类型

~~~html
<div id='app'>
    <h2>{{ message }} vuejs</h2>  <!-- hello vuejs -->
    <h2 v-text='message'>vuejs</h2> <!-- hello -->
</div>
<script>
	const app = new Vue({
        el:'#app',
        data:{
            message:'hello'
        }
    })
</script>
~~~



#### v-pre

+ 用于跳过这个元素和它子元素的编译过程，用于显示原本的Mustache语法

~~~html
<div id='app'>
    <h2 v-pre>{{ message }}</h2> <!-- {{ message }} -->
</div>
<script>
	const app = new Vue({
        el:'#app',
        data:{
           message:"hello"
        }
    })
</script>
~~~



#### v-cloak

+ 某些情况下，浏览器可能会直接显示出未编译的Mustache标签
+ cloak（斗篷）
+ 在vue解析之前，div中有一个属性v-cloak
+ 在vue解析之后，div中没有一个属性v-cloak

~~~html
<style>
    [v-cloak]{
        display:none;
    }	
</style>

<div id='app' v-cloak>
    {{ message }}
</div>

<script>
	setTimeout(function(){
        const app = new Vue({
            el:'#app',
            data:{
                message:'hello'
            }
        })
    })
</script>
~~~



## 2.v-bind动态绑定(p14-19)

### 2.1 v-bind绑定基本属性

+ 前面学习的指令主要作用是将值插入到我们的<font color='red'>模板的内容</font>中
+ 除了内容需要动态决定外，某些属性也希望能动态绑定
+ 该指令v-bind：
  + 作用：动态绑定属性
  + 缩写：`:`
  + 预期：any（with argument）| Object（without argument）
  + 参数：attrOrProp（optional）

~~~html
<div id='app'>
	<img src='{{imgsrc}}'>  <!-- 直接将{{imgsrc}}赋值给src了 -->
    <img src='imgsrc'> <!-- 同上理 -->
    <img v-bind:src='imgsrc'> <!-- 这样才会解析vue中的imgsrc -->、
    <img :src='imgsrc'>  <!-- 语法糖 -->
</div>
~~~



### 2.2 v-bind动态绑定class（对象语法）

对象语法的含义是：class后面跟的是一个对象

+ **用法一：直接通过`{}`绑定一个类**

~~~html
<h2 :class="{'active':isActive}">Hello World</h2>
~~~

+ **用法二：可以通过判断，传入多个值**

~~~html
<h2 :class="{'active':isActiven,'line':isLine}">Hello World</h2>
~~~

+ **用法三：和普通的类同时存在，并不冲突**

~~~html
<h2 class="title" :class="{'active':isActiven,'line':isLine}">Hello World</h2>
~~~

+ **用法四：如果过于复杂，可以放在一个methods或者computed中**

~~~html
<h2 class="title" :class="classes">Hello World</h2> <!-- classes是一个计算属性 -->

<div id='app'>
    <!-- <h2 v-bind:class="{类名1:boolean, 类名2: boolean}"></h2>  -->
    <h2 class='title' v-bind:class="{active:isActive,line:isLine}"></h2>
	<h2 class='title' v-bind:class="getClasses()"></h2>    
</div>

<script>
	const app = new Vue({
        el:"#app",
        data:{
            isActive:true,
            isLine:true
        },
        methods:{
            getClasses:function(){
                return {active:this.isActive,line:this.isLine}
            }
        }
    })
</script>
~~~



### 2.3 v-bind动态绑定class（数组语法）

~~~html
<h2 class='title' :class"['active','line']"></h2> <!-- 与class=“title active line” 同 -->
<h2 class='title' :class"[active,line]"></h2>  <!-- 此时就是变量 -->
<h2 class='title' :class"getClasses()"></h2> 

<script>
	const app = new Vue({
        el:"#app",
        data:{
            active:'aaa',
            line:'bbb'
        },
        methods:{
            getClasses:function(){
                return [this.active,this.line]
            }
        }
    })
</script>
~~~



### 2.4 v-bind绑定style（对象语法）

+ style后面跟的是一个对象类型

~~~html
<h2 :style="{key(属性名,可以不加''):value(属性值,要加'')}">{{message}}</h2>

<h2 :style="{'font-size'(或者fontSize、-要加''):'50px'(if无''会看作变量报错)}">{{message}}</h2>

<h2 :style="{font-size:finalSize(当成变量使用/finalSize2+'px')}">{{message}}</h2>

<h2 :style="grtStyles()">{{message}}</h2>

<script>
	const app = new Vue({
        el:"#app",
        data:{
            finalSize:"50px",
            finalSize2:100,
            finalColor:"red"
        },
        methods{
    		getStyles:function(){
        		return {fontSize:this.finalSize,color:this.finalColor}
    		}    
    	}
        
    })
</script>

~~~



### 2.5 v-bind绑定style（数组语法）

+ style后面跟的是一个数组类型
+ 多个值以‘,’分割

 ~~~html
<div v-bind:style="[baseStyle,baseStyle2]"></div>
<script>
	const app = new Vue({
        el:"#app",
        data:{
            baseStyle:{color:"red"},
            baseStyle:{fontSize:'100px'}
        }  
    })
</script>
 ~~~



## 3. 计算属性(p20-25)

### 3.1 基本使用

~~~html
<div id="app">
	<h2>{{firstname + ' ' + lastname}}</h2>
    <h2>{{firstname}} {{lastname}}</h2>
    
    <h2>{{getfullname()}}</h2>
    
    <h2>{{fullname}}</h2>  <!-- 计算属性 是方法 但是也当作属性看 -->
</div>
<script>
	const app = new Vue({
        el:"#app",
        data:{
            firstname:'lebron',
            lastname:'james'
        },
        computed:{
            //当作属性看 所以方法的命名可以是属性的命名
            fullname:function(){
                return this.firstname + ' ' + this.lastname;
            }
        },
        methods:{
            //当作属性看 所以方法的命名可以是属性的命名
            getfullname:function(){
                return this.firstname + ' ' + this.lastname;
            }
        }
    })
</script>

~~~

### 3.2 复杂操作

~~~html
<div id="app">
	<h2>书的总价 {{totalprice}}</h2>
</div>
<script>
	const app = new Vue({
        el:"#app",
        data:{
            books:[
                {id:11,name:"红宝书",price:119},
                {id:12,name:"红宝书2",price:100},
                {id:13,name:"红宝书3",price:95},
                {id:14,name:"红宝书4",price:45}
            ]
        },
        computed:{
            //当作属性看 所以方法的命名可以是属性的命名
            totalprice:function(){
                let res = 0;
                for(let i = 0 ; i < this.books.length ; i++){
                    res += this.books[i].price
                }
                return res;
            }
        }
    })
</script>
~~~

### 3.3 计算属性的setter和getter

+ 每个计算属性都包含一个setter和getter
+ 上述例子只使用了getter
+ 某些情况下也会需要setter方法（不常用）
+ 例如：

~~~html
<div id="app">    
    <h2>{{fullname}}</h2>  <!-- 就算是有get set 也不加() -->
</div>
<script>
	const app = new Vue({
        el:"#app",
        data:{
            firstname:'lebron',
            lastname:'james'
        },
        computed:{
            fullname:{
                set:function(newname){
                    const names = newname.split(' ');
                    this.fristname = names[0];
                    this.lastname= names[1];
                },
                get:function(){
                    return this.firstname + ' ' + this.lastname;
                }
            }
            
            //因为一般只有get属性 只读 所以就可以简写成下面
          	//fullname:{
            //    get:function(){
            //        return this.firstname + ' ' + this.lastname;
            //    }
            //}
            //fullname:function(){
            //    return this.firstname + ' ' + this.lastname;
            //}
        }
    })
</script>
~~~

### 3.4 计算属性的缓存

计算属性和methods的区别 

在多次调用时：

+ 计算属性只会调用一次，内部有缓存，如果值不变，就不会再调用，会直接使用，性能比较高
+ methods会调用多次，每次都会重新计算，性能较低

所以更多使用计算属性会比较好



## 4.es6补充（p26-29）

### 4.1 let/var 块级作用域

+ 变量作用域：变量能够使用的范围

  + 没有块级作用域引起的问题：if的块级

  + 没有块级作用域引起的问题：for的块级

    ~~~js
    var btns = document.querySelector('button');
    for(var i = 0; i < btns.length; i++){
         //因为i的作用域问题  所以会i一直都是btns.length
        btns[i].addEventListener("click",function(){
            console.log('第' + i + '个按钮被点击' ); 
        })
        
        //解决一：闭包（立即执行函数）
        //原因：函数是一个作用域
        (function(i){
         	btns[i].addEventListener("click",function(){
            	console.log('第' + i + '个按钮被点击' ); 
        	})
        })(i)
        
    }
    ~~~

  

+ es5之前因为if和for都没有块级作用域的概念，所以再很多时候，都必须借助于function的作用域来解决应用外面变量的问题

+ es6中，let有if和for的块级作用域

### 4.2 const的使用

es6开发中，优先使用const，只有需要改变一个标识符时才使用let

+ 给const修饰的标识符被赋值后，不能再改变
+ 再使用const定义标识符，必须进行赋值
+ 常量的含义是指向对象不能修改，但是对象内部的属性可以修改



### 4.3 对象字面量的增强写法

~~~js
const obj = new Object();

//字面量
const obj = {
    
}
~~~

#### 1.属性的增强写法

~~~js
const name = 'why';
const age = 18;
const height = 1.88;

//es5的写法
const obj = {
    name:name,
    age:age,
    height:height
}

//es6的写法
const obj ={
    name,
    age,
    height
}
~~~

#### 2.函数的增强写法

~~~js
//es5的写法
const obj = {
    run:function(){
        
    },
    eat:function(){
        
    }
}

//es6的写法
const obj = {
    run(){
        
    },
    eat(){
        
    }
}
~~~



## 5. v-on事件监听（p30-32）

### 5.1 v-on基本使用

+ 作用：绑定事件监听器

+ 缩写：@

+ 预期：Function|Inline Statement|Object

+ 参数：event

  

### 5.2 v-on参数问题

+ $event

~~~html
<!-- 1.事件调用的方法没有参数 ()可省略-->
<button @click="btnClick()">按钮1</button>
<button @click="btnClick">按钮1</button>

<!-- 2.有参数时,若只写()没有传参 那么与普通对象一样 形参为undefined -->
<!-- 若没有写() vue会默认将浏览器生产的event事件对象作为参数传入方法 -->
<button @click="btn2Click(123)">按钮2</button>
<button @click="btn2Click()">按钮2</button>
<button @click="btn2Click">按钮2</button>

<!-- 3.在方法定义时，需要参数又需要event -->
<!-- 在调用方法，手动获取到浏览器参数的event对象：$event -->
<button @click="btn3Click(123,$event)">按钮3</button>

<script>
 const app = new Vue({
     el:"#app",
     data:{},
     methods:{
         btnClick(){
             
         },
         btn2Click(abc){
             console.log("----",abc);
         },
         btn3Click(abc,event){
             console.log(abc,event);
         }
     }
 })
</script>
~~~



### 5.3 v-on 修饰符

+ .stop - 调用event.stopPropagation()
+ .prevent - 调用event.preventDefault()
+ .{keyCode | keyAlias} -只当事件是从特定键触发时才触发回调
+ .native - 监听组件根元素的原生事件
+ .once - 只触发一次回调

~~~html
<div id='app'>
    <!-- 1..stop修饰符的使用 -->
	<div @click="divClick">
        <!-- if 不加 .stop 点击按钮也会点击到div 会触发事件冒泡 .stop 阻止事件冒泡-->
        <button @click.stop='btnClick'>按钮</button>
    </div>
    
    <!-- 2..prevent -->
    <form action="baidu">
        <!-- if 不加 会自动提交表单 -->
        <input type='submit' value='提交' @click.prevent='submitClick'>
    </form>
    
    <!-- 3. 监听某个键盘的键帽 keyup 松开键盘的时候触发-->
    <input type="text" @keyup.enter="keyup">
    
    <!-- 4. .once 只会触发一次-->
    <button @click='btnClick'>按钮</button>
</div>

<script>
	const app = new Vue({
        el:"#app",
        methods:{
            btnClick(){
                console.log("btnClick");
            },
            divClick(){
                console.log("divClick");
            },
            submitClick(){
                console.log("submitClick");
            },
            keyup(){
                console.log("keyup");
            }
        }
    })
</script>
~~~



## 6.条件判断v-if、v-else-if、v-else、v-show(p33-36)

+ 这三个指令与JavaScript的条件语句if、else、else if、类似
+ Vue的条件指令可以根据表达式的值在DOM中渲染或销毁元素或组件

+ v-if的原理：
  + v-if的值为false时，对应的元素及其子元素不会渲染
  + 不会有对应的标签出现在DOM中

### 6.1 v-if、v-else

~~~html
<div id="app">
    <h2 v-if="isShow">isShow为true时显示</h2>
    <h1 v-else>isShow为false时显示</h1>
    
    <!-- 如果有这样的情况 建议用computed做 不建议使用这么复杂的标签做 -->
    <p v-if="score >= 90">优秀</p>
    <p v-else-if="score >= 80">良好</p>
    <p v-else-if="score >= 60">及格</p>
    <p v-else>不及格</p>
</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            isShow:true,
            score:99,
        }
    })
</script>
~~~

### 6.2 账号/邮箱登录 input复用的问题解决（p34-p35）

+ 在两个input上+不同的key的值就可以了
+ Vue ：在Dom渲染页面的时候会经过一层虚拟dom然后将相似的不会冲突的元素只渲染一遍
+ 所以用不同的key把两个分开
+ 这样就不会出现切换input时 input里面的value还存在的问题了



### 6.3 v-show

v-if：为false时，不会存在dom，删除/重新创建

v-show：为flase，display：none；block/none    切换频率高时，使用





## 7.循环遍历（p37-44）

### 7.1 v-for 遍历数组和对象

1. 数组: `<p v-for="(item,index) in arr">{{ index+1 }} {{ item }}</p>`

2. 对象:`<li v-for="(value,key,index) in info"></li>`



### 7.2 v-for绑定与不绑定key的区别

##### 插入函数 splice : 

~~~js
let letters = ["A","B","C","D","E"];
letters.splice(2,2); //返回["C","D"]
// letters = ["A","B","E"];

let letters = ["A","B","C","D","E"];
letters.splice(2,0,"F");  //返回[]
//["A","B","F","C","D","E"]
~~~

##### 绑定key的格式：

~~~html
<li v-for="item in letters" :key="item">{{ item }}</li>
~~~

##### 组件的key属性

+ 官方推荐在使用`v-for`时，给对应的元素/组件添加上`:key`属性

+ >  2.2.0+ 的版本里，**当在组件中使用** v-for 时，key 现在是必须的。因为没有key来保障循环中的唯一性，那么组件则会被打乱。
  >
  > 当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用 “**就地复用**” 策略。如果数据项的顺序被改变，Vue将**不是移动 DOM 元素来匹配数据项的顺序**， 而是**简单复用此处每个元素**，并且确保它在特定索引下显示已被渲染过的每个元素。
  >
  > 为了给 Vue 一个提示，**以便它能跟踪每个节点的身份，从而重用和重新排序现有元素**，你需要为每项提供一个唯一 key 属性。

+ 原因：和Vue的虚拟DOM的Diff（顺序表的插入）算法有关

+ 所以我们需要使用key来给每个节点做一个唯一标识符，Diff算法就可以正确的识别此节点，找到正确的位置区插入新的节点

+ key的作用主要是为了高效的更新虚拟DOM

### 7.3 那些数组的方法是响应式的

1. push() ：可以跟多个
2. pop()  :删除数组最后一个元素
3. shift()：删除数组的第一个元素
4. unshift()：在第一个前面增加元素 ，可以传多个值
5. splice()：删除/插入/替换元素
6. sort()：
7. reverse()：

+ <font color='red'>注意：通过索引值修改数组中的元素 这个方法不是响应式的</font>

  > 页面的元素不会响应式地改变 但是控制台上面的数据就换了

  + 可以使用splice 
  + 使用Vue.set(this.letters,0,'bbbb');



### 7.4 作业+案例

+ 保留两位小数 `{{ item.price.toFixed(2) }}`
+ 过滤器： （使用过滤器做这个功能会比较适合）

~~~html
<td>{{ '￥' + item.price.toFixed(2) }}</td>
<td>{{ getFinalPrice(item.price) }}</td>
<!-- <td>{{ item.price | 过滤器 }}</td>  过滤器是有参数的 直接把|前面的作为参数传入  --> 
<td>{{ item.price | showPrice }}</td>

<script>
	const app = new Vue({
        el:"#app",
        data:{
            book:[...],
        },
        methods:{
        	getFinalPrice(price){
            	return '￥' + price.toFixed(2);
        	}          
        },
        filters:{
        	showPrice(price){
        		return '￥' + price.toFixed(2);
    		}                
        }
             
    })
</script>
~~~



## 8. JavaScript高阶函数的使用（p45）

### 8.1 循环

+ for循环
+ for-in
+ for-of

### 8.2filter/map/reduce

+ filter中的回调函数有一个要求：必须返回一个boolean值

+ true时：函数内部会自动将这次回调的n加入到新的数组中

+ false时：函数内部会过滤掉这次的n

  ~~~js
  const nums = [10,20,50,111,5555,12];
  
  //选出<100的数 * 2 相加
  
  //1.filter 的使用
  let newNums = nums.filter(function(n){
      return n<100;
  })
  console.log(newNums)
  
  //2.map的使用
  let new2Nums = newNums.map(function(n){
      return n*2;
  })
  console.log(new2Nums)
  
  //3.reduce的使用
  //对数组的全部内容进行汇总 第二个参数是初始值
  let total = new2Nums.reduce(function(preValue,n){
  	return preValue + n;
  },0)
  
  
  //全部汇总 --> 函数式编程
  let total = nums.filter(function(n){
      return n<100;
  }).map(function(n){
      return n*2;
  }).reduce(function(preValue,n){
      return preValue + n
  },0)
  
  //使用简写
  let total = nums.filter(n => n<100).map(n => n*2).reduce((pre,n) => pre +n );
  ~~~



## 9.表单绑定v-model(p46-51)

+ Vue中使用v-model指令来实现表单元素和数据的双向绑定

### 9.1 v-model 原理

+ v-model其实是一个语法糖，它的本质是包含两个操作：

  1. v-bind绑定value属性
  2. v-on指令给当前元素绑定input事件

  ~~~html
  <input type='text' v-model="message">
  <!-- 等于 -->
  <input type='text' v-bind:value="message" v-on:input="message=$event.target.value">
  <!-- 等于 -->
  <input type='text' :value="message" @input="valueChange">
  
  <script>
  	const app = new Vue({
          el:"#app",
          data:{
              message:"hello",
          },
          methods:{
              valueChange(event){
                  this.message = event.target.value;
              }
          }
      })
  </script>
  ~~~

### 9.2 v-model 结合radio类型

~~~html
<div id="app">
    <!-- 因为使用了v-model所以可以不使用name -->
    <label for='male'>
    	<input type="radio" id="male" value="男" v-model='sex'>男
    </label>
    <label for='female'>
    	<input type="radio" id="female" value="女" v-model='sex'>女
    </label>
    <!-- 因为vue里的sex有值 所以会有默认选上的 -->
    <h2>您选择的性别为：{{ sex }}</h2>
</div>

<script>
    const app = new Vue({
        el:"#app",
        data:{
            message:"hello",
            sex:'女'
        }
    })
</script>
~~~

### 9.3 v-model 结合checkbox类型

~~~html
<div id="app">
    <!-- 1.checkbox 单选框 -->
	<label for="agree">
        <input type="checkbox" id="agree" v-model="isAgree">同意协议
    </label>
    <h2>您的选择是:{{ isAgree }}</h2>
    <button :disable="!isAgree">下一步</button>
    
    <!-- 2.checkbox 多选框 -->
    <input type="checkbox" value="篮球" v-model="bobbies">篮球
    <input type="checkbox" value="足球" v-model="bobbies">足球
    <input type="checkbox" value="兵乓球" v-model="bobbies">兵乓球
    <input type="checkbox" value="羽毛球" v-model="bobbies">羽毛球
    <h2>您的爱好是：{{ hobbies }}</h2>
</div>
<script>
	let app = new Vue({
        el:"#app",
        data:{
            message:'hello',
            isAgree:false,
            hobbies:[]
        }
    })}
</script>
~~~

### 9.4 v-model结合select类型

~~~html
<div id='#app'>
    <!-- 1.选择一个 -->
    <select name="abc" v-model="fruit">
        <option value="苹果">苹果</option>
        <option value="香蕉">香蕉</option>
        <option value="榴莲">榴莲</option>
        <option value="葡萄">葡萄</option>
    </select>
    <h2>您选择的水果是：{{fruit}}</h2>
    
    <!-- 2.选择多个 -->
    <select name="abc" v-model="fruits" multiple>
        <option value="苹果">苹果</option>
        <option value="香蕉">香蕉</option>
        <option value="榴莲">榴莲</option>
        <option value="葡萄">葡萄</option>
    </select>
    <h2>您选择的水果是：{{fruit}}</h2>
</div>

<script>
	let app = new Vue({
        el:"#app",
        data:{
            message:'hello',
            friut:"香蕉", //所以香蕉会默认选上
            fruits:[]
        }
    })}
</script>
~~~

### 9.5 值绑定

+ 与v-bind相似

~~~html
<label v-for="item in originHobbies" :for="item">
	<input type="checkbox" :value:"item" :id="item" v-model="hobbies">{{item}}
</label>
~~~

### 9.6 修饰符

##### .lazy修饰符

+ 默认情况下，v-model默认是在input事件中同步输入框的数据的

  > 即一旦有数据改变 data中的数据就会自动发生改变

+ lazy修饰符可以让数据在失去焦点或回车时才会更新

##### .number修饰符

+ 默认情况下，在输入框中输入的内容，都会被当作字符串类型进行处理
+ 如果希望处理的是数字类型 type=”number“的，那么最好直接将内容当作数字处理
+ number修饰符可以让输入框中输入的内容自动转换成数字类型

##### .trim修饰符

+ 如果输入的内容首尾有很多空格，通常希望将其去除
+ trim修饰符可以过滤内容左右两边的空格



~~~html
<!-- lazy -->
<input type="text" v-model.lazy="message">

<!-- number -->
<input type="number" v-model.number="age">

<!-- trim -->
<input type="text" v-model.trim="name">
~~~

















