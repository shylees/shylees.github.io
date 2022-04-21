---
title: 05 Vue CLI
date: 2021/3/21
updated: 2021/3/23
comments:
tags:
 - vue
 - es6
categories:
 - learningNotes
layout:
permalink:
meta: true
---

[TOC]

## 1.Vue CLI介绍和安装

### 1.1 CLI 是什么意思？

+ CLI是Command-Line Interface ，即命令行界面，俗称脚手架
+ Vue CLI是一个官方发布vue.js项目脚手架
+ 使用vue-cli可以快速搭建Vue开发环境以及对应的webpack配置

### 1.2 Vue CLI使用前提 - Node

node环境要求8.9以上版本

### 1.3 Vue CLI使用前提 - Webpack

Vue.js官方脚手架工具使用了webpack模板

### 1.4 Vue CLI的使用

1. 安装Vue脚手架 在cmd终端安装

   > ~~~js
   > npm install -g @vue/cli    //版本3.2.1
   > 
   > vue --version //查看版本  //装了4.5.12
   > ~~~

   因为安装的是Vue CLI3的版本 如果想按照Vue CLI2的方式初始化项目是不可以的

2. 拉取2.x版本

   > Vue CLI3和 旧版使用了相同的`vue`命令 ，所以Vue CLI2(`vue-cli`)被覆盖了，如果你仍需使用旧版本的 `vue init`功能，可以全局安装一个桥接工具：
   >
   > ~~~js
   > npm install -g @vue/cli-init  //拉取脚手架2 使用脚手架2
   > vue init webpack my-project   //使用2 创建项目
   > ~~~

3. Vue CLI2 初始化项目

   ~~~js
   vue init webpack my-project
   ~~~

4. Vue CLI3初始化项目

   ~~~js
   vue create my-project
   ~~~



### 1.5 vuecli-CLI2初始化项目过程

1. In   : `vue init webpack vuecli2test` //项目名字不能大写

2. 下载配置

3. Out: `? Project name (vuecli2test) `    //一般文件名 跟项目名一样 

4. Out:`? Project description (A Vue.js project)` //项目描述

   In    :`test vue cli2`

5. Out:`? Author (piaoliangjiejie2019 <piaoliangjiejie2019@outlook.com>)` //全局git

6. Out:`Runtime + Compiler: recommended for most users
   Runtime-only: about 6KB lighter min+gzip, but templates (or any Vue-specific HTML) are ONLY allowed in .vue files - render functions are required elsewhere` //询问要用哪个构建项目

   In   :  //选择下面个  打包出的小 运行效率高

7. Out:`? Install vue-router? (Y/n) `  //是否安装路由 暂时not 因为还没学

8. Out:`? Use ESLint to lint your code? (Y/n) ` //对js的限制 规范代码 if不规范会报错

   In   :`y`

   Out:`Standard (https://github.com/standard/standard)
   Airbnb (https://github.com/airbnb/javascript)
   none (configure it yourself)` //哪个人的规范

   In   : //暂时选标准

9. Out:`? Set up unit tests (Y/n) `  //单元测试 用的少 n

10. Out:`? Setup e2e tests with Nightwatch? (Y/n) ` //e to e ->end to end 端到端  n

11. Out:`? Should we run npm install for you after the project has been created? (recommended) (Use arrow keys)`

    `Yes, use NPM
    Yes, use Yarn
    No, I will handle that myself `  //使用 npm / yarn

    In  : `npm`

<img src='https://s1.328888.xyz/2022/04/09/Xaimv.jpg'>

### 1.6 vuecli-CLI2目录的解析.

+ node test.js 命令 可以直接在终端输出 控制台的内容

<img src='https://s1.328888.xyz/2022/04/09/Xaro0.jpg'>



## 2.ESLint规范和runtime compiler/only区别

### 2.1 eslint

+ 开了两个项目 01runtimecompiler 装了eslint   02runtimeonly 没装eslint

  > eslint 有很多奇奇怪怪的标准  保存就会显示报错信息

  > 选了后 想关掉eslint ：congif->index.js-> 26 行的 useEslint 改成 false 然后编译即可

###  2.2 runtime compiler和only的区别（p96）

1. 区别只在main.js里

   + runtime compiler：

     ~~~js
     import Vue from 'vue'
     import App from './App'
     
     Vue.config.productionTip = false
     
     /* eslint-disable no-new */
     new Vue({
       el: '#app',
       components: { App },
       template: '<App/>'
     })
     ~~~

   + runtime only：

     ~~~js
     import Vue from 'vue'
     import App from './App'
     
     Vue.config.productionTip = false
     
     /* eslint-disable no-new */
     new Vue({
       el: '#app',
       render: h => h(App) // 相当于
         //render:function(h){return h(App)}
     })
     ~~~

2. vue运行过程

   <img src='https://s1.328888.xyz/2022/04/09/XaA7F.png' style="zoom:50%;" >

3. 对比：

   + runtime-compiler ：

     > template -> ast  -> render -> vdom -> UI

   + runtime-only（性能更高，代码量更少）

     >render -> vdom ->UI

   + 所以尽量使用 runtime-only

4. render 函数 和 createElement函数

   ~~~js
   //runtime-compiler
   
   render: function(createElement) {
           //1.普通用法： createElement('标签',{标签的属性},['']) 
           //返回的东西 会覆盖 #app的内容
           // return createElement('h2', { class: 'box' }, ['hello']);
   
           //2.传入组件对象
           //这样的话 就是直接使用render函数 省去了前两步内容 效率更高
           // return createElement(cpn);
           return createElement(App);
       }
   ~~~

   + 在runtime-only main.js 里 App 已经没有包含 template 了

     > .vue 文件中的template是由 vue-template-compiler ——将.vue文件的template解析成render函数

     

5. 总结：

   + 如果之后的开发中 依然使用template，就需要选择runtime-compiler
   + 如果使用的是.vue文件 就可以选择runtime-only



## 3.Vue CLI3(p97-)

### 3.1 vue-cli 3 与 2 的区别

+ vue-cli 3 是基于webpack 4 打造的，vue-cli 2 还是 webpack 3
+ vue-cli 3 的设计是‘0配置’ ，移除的配置文件根目录下的 build和config等目录
+ vue-cli 3 提供了vue ui 命令，提供了可视化配置，更加人性化
+ 移除了static文件夹，新增了public文件夹，并且index.html移动到public中

### 3.2 创建项目

In：`vue create 03vuecli3test `

Out：`Vue CLI v4.5.12`
`? Please pick a preset: (Use arrow keys)`   //选择配置
`>Default ([Vue 2] babel, eslint)  //默认2
Default (Vue 3 Preview) ([Vue 3] babel, eslint) //默认3
Manually select features`  //手动  √ 

Out：  //按空格是 选择 / 取消

`(*) Choose Vue version  //后面选3.x
 (*) Babel
 ( ) TypeScript
 ( ) Progressive Web App (PWA) Support  //先进app 可以缓存很多东西 也有推送通知
 ( ) Router
 ( ) Vuex
 ( ) CSS Pre-processors  //css预处理器 if用less什么的就可以选
 (*) Linter / Formatter   //eslint
 ( ) Unit Testing   //测试
 ( ) E2E Testing`   //测试

Out：`? Where do you prefer placing config for Babel, ESLint, etc.? (Use arrow keys) `   

`In dedicated config files
In package.json`  //配置文件的存放位置  独立文件 /pack.json  选单独的

Out：`? Save this as a preset for future projects? (y/N)` //是否保存配置 会添加到第1个out  y

> 如果想删掉 在 users/lsy/.vuerc   里面有presets 对象 然后删掉里面的 值就可以了

Out：`? Save preset as: ` //保存的名字  coedrwhy

### 3.3 跑项目

#### 3.3.1 cli 2

<img src='https://s1.328888.xyz/2022/04/09/XaHJy.png'>

<img src='https://s1.328888.xyz/2022/04/09/XaqWk.png'>

#### 3.3.2 cli3

看package.json  的scripts的东西  

里面是 serve   & build 

所以是 npm run serve 开发/ build 发布  

~~~js
import { createApp } from 'vue'
import App from './App.vue'

createApp(App).mount('#app')

//相当于 
// new Vue({
//     el: "#app",
//     render: function(h) {
//         return h(App)
//     }
// })
~~~



### 3.4 Vue-Cli3配置文件的查看和修改

+ UI方向的配置

  + 启动配置服务器：vue ui  //启动本地服务器 不用进入哪个地址
  + 导入 刚建的文件夹  
  + 左2 插件 左3 依赖 左4 配置 可以改的 左5 任务 可以各种运行

+ src+文件 vue.config.js

  ~~~js
  module.exports={}   //会跟其他的配置文件合并一起的 放独有的配置
  ~~~



## 4.箭头函数的使用和this的指向问题(p99)

### 4.1 基本使用

~~~js
//1.定义函数的方式：function
const aaa = function(){
    
}
//2.对象字面量中定义函数
const obj = {
    bbb:function(){
        
    },
    bbb(){
        
    }
}
//3.ES6中的箭头函数
const ccc = (参数列表) =>{
    
}

const aaa = () =>{
    
}
~~~



### 4.2 箭头函数参数和返回值

~~~js
//1.参数问题
//1.1放入两个参数
const sum =(num1,num2) =>{
    return num1+num2;
}

//1.2 放一个参数
const power = (num) =>{  //括号可以省略
    return num*num;
}

//2.返回值
//2.1 函数代码块中有多行代码
const test = () =>{
    //1.打印Hello World
    console.log('Hello World');
    
    //2.打印Hello Vue
    console.log('Hello Vue');
    
}

//2.2 函数代码块中只有一行代码
const mul = (num1,num2) =>{
    return num1+num2;
}

//等同于
const mul2 = (num1,num2) => num1*num2

//无返回值时 自动将结果作为返回值 给demo
const demo = () => console.log('Hello vue')
~~~



### 4.3 箭头函数的this的使用

+ 使用箭头函数多：当准备一个函数作为参数传给另一个函数的时候

~~~js
setTimeout(function(){
    console.log(this);  //window
},1000)

setTimeout(()=>{
    console.log(this); //window
},1000)

//结论：箭头函数中this引用的就是最近作用域中的this
const obj ={
    aaa(){
        setTimeout(function(){
            console.log(this); //window
        })
        
        setTimeout(()=>{
            console.log(this); //obj对象
        })
    }
}


const obj ={
    aaa(){
        setTimeout(function(){
            setTimeout(function(){
                console.log(this); //window
            })
            
            setTimeout(()=>{
                console.log(this); //window
            })
        })
        
        setTimeout(()=>{
            setTimeout(function(){
                console.log(this); //window
            })
            
            setTimeout(()=>{
                console.log(this); //obj
            })
        })
    }
}
~~~

