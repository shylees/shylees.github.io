---
title: 09 Vuex
date: 2021/4/21
updated: 2021/5/2
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


## 1.Vue的概念和作用解析

### 1.1 Vuex 是做什么的

官方解释：Vuex是一个专门为Vue.js应用程序开发的状态管理模式。

采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

Vuex也集成到Vue的官方调试工具`devtools extension`，提供了诸如零配置的`time-travel`调试、状态快照导入导出等高级调试功能。

> 状态管理模式相当于 封装一个大家都能访问的对象里 但Vuex跟自己封装的对象不一样的就是Vuex 实现了响应式
>
> `Vuex就是为了提供这样一个在多个组件间共享状态的插件`



### 1.2 管理什么状态呢？

比如：用户的登录状态、用户名称、头像等

比如：商品的收藏、购物车中的物品等

这些状态信息，我们都可以放在统一的地方，对它进行保存和管理，而且他们还是响应式的



### 1.3 单页面的状态管理

<img src='https://s1.328888.xyz/2022/04/09/XaTeC.png' style="zoom:67%;" >

State：状态—data中的属性

View：视图层，可以针对State的变化，显示不同的信息—模板

Actions：用户的各种操作，会导致状态的改变—方法



### 1.4 多界面状态管理

Vuex–全局单例模式 （大管家）—Vuex背后的思想

+ 将共享的状态抽取出来，交给大管家，统一管理
+ 每个视图，按照规定好的规定，进行访问和修改等操作



### 1.5 Vuex状态管理图例

<img src='https://s1.328888.xyz/2022/04/09/XacDg.png' style="zoom:50%;" >

Devtools ：记录每一次status修改的状态，所以官方网站推荐改vuex时，在这里改

如果绕过mutations，就没法跟踪修改的状态

> mutations 同步操作，所以只有同步操作时可以绕过actions
>
> actions 异步操作时 如网络请求

backend：后端



### 1.6 安装devtools插件

在f12中 的更多 如果又看到vue 就安装成功

## 2. Vuex的使用

### 2.1 Vuex的下载

~~~js
npm install vuex --save @3.0.1
~~~



### 2.2 Vuex使用

1. 把Vuex插件导入

   ~~~js
   //main.js
   
   import Vuex from 'vuex'
   
   //1.安装插件
   Vue.use(Vuex)
   
   //2.创建实例
   
   ~~~

   一般不在main中写，重建文件夹，store，创建index.js

   > 导入模块   安装插件 创建store对象 导出对象   在main挂载Vue.prototype.$store = store
   >
   > 在state保存状态
   >
   > 在mutations写方法 修改state
   
   ~~~js
   import Vue from 'vue'
   import Vuex from 'vuex'
   
   //1.安装插件
   Vue.use(Vuex)   //底层会执行这个插件的方法 ： Vuex.install
   
   //2.创建对象
   const store = new Vuex.Store({
       state:{
           counter:1000
       },     //保存状态
       mutations: {
           //方法
           increment(state) {
               state.counter++;
           },
           decrement(state) {
               state.counter--;
           }
       },
       actions:{},
       getters:{},
       modules:{}
   })
   
   //3.导出store对象
   export default store
   
   //在main.js挂载
   import stroe from './stroe'
   
   //这样才会 多个组件使用它
   Vue.prototype.$store = store
   
   ~~~
   
   在其他页面使用counter 
   
   > 使用state的状态 $store.state.counter
   >
   > 使用里面的方法 $store.commit(“方法名”)
   
   ~~~vue
   //app.vue
   <template>
   	<h2>------app内容：直接compontens 修改 state -------</h2>
   	<h2>{{$store.state.counter}}</h2>
   	<button @click='$store.state.counter++'>+</button>
   
   	<h2>--------vuex内容：使用nutations--------</h2>
   	<button @click='sub'>-</button>
   
   </template>
   <script>
   import HelloVuex from "./components/HelloVuex";
   export default {
   	name: "App",
   	components: {
   		HelloVuex,
   	},
   	data() {
   		return {
   			message: "我是app组件",
   			// counter: 0,
   		};
   	},
   	methods: {
   		add() {
   			return this.$store.commit("increment");
   		},
   		sub() {
   			return this.$store.commit("decrement");
   		},
   	},
   };
   </script>
   ~~~
   
   <img src='https://s1.328888.xyz/2022/04/09/Xamw1.jpg' style="zoom: 50%;" >



## 3.Vuex核心概念（p133 2021/05/01)

### 3.1 State 单一状态树 

> 相当于data

一个项目就一个store

### 3.2 Getters

> 相当于computed

与计算属性相似，如果要发生什么变化再给其他组件使用

1. 如果要对数据产生什么变化，就直接在getters变化后，去使用就可以了

   + 在getters定义计算属性

     ~~~js
     getters(){
         powCounter(state){
             return state.counter * state.counter;
         }
     }
     ~~~

   + 在app的template使用计算属性  因为是计算属性 所以可以不用加括号

     ~~~vue
     <template>
     	<h3>原来的方法：{{$store.state.counter * $store.state.counter}}</h3>
     	<h3>使用getters:{{$store.getters.powCounter}}</h3>
     </template>
     ~~~

   

2.  如果要使用对象中的某一个筛选对象，例如学生中选年龄大于12岁的,加学生个数，传入年龄 选出年龄大的学生

   + ~~~js
     // index.js 
     state: {
             //保存状态
             counter: 1000,
             students: [
                 { id: 111, name: 'why1', age: 11 },
                 { id: 112, name: 'why2', age: 12 },
                 { id: 113, name: 'why3', age: 13 },
                 { id: 114, name: 'why4', age: 14 },
             ]
         },
     ~~~

   + > 使用计算属性
     >
     > ~~~vue
     > <template>
     > 	<h3>{{more12stu}}</h3>
     > </template>
     > <script>
     > computed: {
     > 		 more12stu() {
     > 		 	return this.$store.state.students.filter((s) => s.age > 12);
     > 		 },
     > 	},
     > </script>
     > ~~~
     >
     > 要是别的页面要用就得复制过去，很麻烦

   + 使用getters

     ~~~js
     //index.js 
     getters: {
             //大于12的学生
             more12stu(state) {
                 return state.students.filter(s => s.age > 12)
             },
             //大于12的学生的个数
             more12stuLength(state,getters) {
                 return getters.more12stu.length;
             },
             //大于age的学生
             moreAgestu(state){
                 return age => state.students.filter(s => s.age > age)
             }
         },
     ~~~

     

   + 使用getters的数据

     ~~~js
     //vue
     <h3>年龄大于20的学生：{{ $store.getters.more12stu }}</h3>
     
     //大于12的学生的个数 可以直接 写length
     <h3>{{ $store.getters.more12stu.length }}</h3>
     //也可以再写一个计算属性
     <h3>{{ $store.getters.more12stuLength }}</h3>
     
     //年龄大于age的学生
     <h3>{{ $store.getters.moreAgestu(11) }}</h3>
     ~~~

     

### 3.3 Mutation

> 相当于methods

+ vuex的store状态的更新唯一方式：commit mutation

+ mutation包括两个部分：

  + 字符串的事件类型type `increment`
  + 一个回调函数handler，该回调函数的第一个参数是state`(state){state.couter--}`

+ mutation的定义方式：

  ~~~js
  //index.js
  mutatuions:{
      increment(state){
          state,counter--
      }
  }
  ~~~

+ mutation的更新：

  ~~~js
  //vue
  add:function(){
      this.$store.commit('increment')
  }
  ~~~

#### 3.3.2 传入参数

**参数被称为mutation的载荷Payload**

点击＋任意数量   添加学生

+ ~~~vue
  //vue
  <button @click='addcount(5)'>+5</button>
  <button @click="addstu">添加学生</button>
  ~~~

+ 写方法

  ~~~js
  //vue
  methods:{
      addcount(count){
          return this.$store.commit('incrementcount',count);
      },
      addstu() {
  	    const stu = { id: 115, name: "why5", age: 15 };
  		return this.$store.commit("addstu", stu);
  	},
  }
  ~~~

+ mutation

  ~~~js
  //index.js
  mutations:{
      incrementcount(state,count){
          state.counter += count;
      },
      addstu(state, stu) {
          state.students.push(stu)
      }
  }
  ~~~



#### 3.3.3 mutation的提交风格

~~~js
//vue methods
addcount(count) {
	//1. 普通提交方式
	 return this.$store.commit("incrementcount", count);
    
	//2.特殊的封装方式
	return this.$store.commit({
		type: "incrementcount",
		count,
	});
},
~~~

~~~js
//index.js mutation 
incrementcount(state, count) {
       console.log(count);
       // 如果是普通提交 返回的是 count的数值
       // 如果是封装的提交 返回的是整个参数对象  
       // 这里就应该使用 payload参数命名 然后使用layload.count
       state.counter += count
},
==>
incrementcount(state,layload){
    state.counter += layload.count;
}
~~~



#### 3.3.4 vuex数据响应式原理(对组件也是一个道理)

<img src='https://s1.328888.xyz/2022/04/09/XawBt.jpg' style="zoom:50%;" >

在里面的定义的每个属性 都会有一个Dep[watcher…]  watcher会监听每一个页面的属性变化



##### 3.3.4.1在mutations增加删除数据事如何做到响应式

~~~js
mutations:{
    updateInfo(state){
        //这样无法响应式 就是代码改了 无法在页面改 因为没有监听
        state.info['address'] = 'china';
        //使用Vue.set方法就可以
        Vue.set(state.info , 'address' , 'china');
        
        //删除数据时 使用delete删除无法响应式
        delete state.info.age;
        //使用Vue.delete可以
        Vue.delete(state.info , 'age');
    }
}
~~~



#### 3.3.5 mutation常量类型

把vue里面methods里面commit括号里的”increment“，和index.js里mutations里的increment变成一个常量，这样不会写错

1. 在store文件夹下面 新建 `mutations.types.js` 用来导出常量

   ~~~js
   //mutations.types.js
   export const INCREMENT = "increment"
   ~~~

2. 导入文件，修改vue里的”increment“

   ~~~js
   import { INCREMENT } from "../store/mutations.types";
   add() {
   	return this.$store.commit(INCREMENT);
   },
   ~~~

3. 导入文件，修改index.js的increment

   ~~~js
   import { INCREMENT } from './mutations.types'
   [INCREMENT](state) {
               state.counter++;
   },
   ~~~

### 3.4 Action

> 相当于异步methods

在mutations中进行异步操作，devtools插件没有办法追踪到变化，页面变化，但是控制台的代码没有变

所以用actions替代mutations

~~~js
//index.js
mutations:{
    updateInfo(state){
        return state.info.name = 'why';
    }
}
actions:{
    //context:上下文  可看成state 不能跳过mutations
    //传参数
    aUpdateInfo(context,payload){
        setTimeOut(() => {
            context.commit('updateInfo',payload); 
            console.log(paylog);
        },1000)
    }
}
~~~

在vue中操作

~~~js
<button @click="updateInfo">更新</button>

updateInfo(){
    //this.$store.commit('updateInfo');  //不会改代码
    this.$store.dispatch('aUpdateInfo','我是payload');  //这样才会经过actions mutations 然后被插件检测到
}
~~~



**在修改成功的时候通知用户**

一般在commit的地方就修改成功了

+ 正常写法：

  ~~~js
  //vue
  updateInfo(){
      //this.$store.dispatch('aUpdateInfo',()=>{
      //    console.log("修改成功");
      //})
      return this.$store.dispatch('aUpdateInfo',{
          message:"我是携带信息",
          success:()=>{
          console.log("修改成功");
      }
      })
  }
  ~~~

  ~~~js
  //index.js
  actions:{
      aUpdateInfo(context,payload){
          setTimeout(() => {
              context.commit('updateInfo');  //一般在这会修改成功
              //payload()
              console.log(layload.message);
              payload.success();
          },1000)
      }
  }
  ~~~

+ 优雅的写法 使用promise

  ~~~js
  //vue
  updateInfo(){
      return this.$store
      .dispatch("aUpdateInfo")
      .then(res => {
          console.log(res)
      })
  }
  ~~~

  ~~~js
  //index.js
  aUpdateInfo(context,payload){
      return new Promise((resolve,reject) => {
          setTimeout(() => {
              context.commit('updateInfo');
              console.log(payload);
              resolve(111);
          })
      })
  }
  ~~~

### 3.5 Module

+ 因为vue使用单一状态树意味着很多状态都给vuex来管理
+ 当应该很复杂时store就很臃肿
+ 所以vuex允许我们将store分割城模块，每个模块拥有自己的state、mutations、actions、getters

~~~js
//index.js
const moduleA = {
    state: {
        name: "lisi"
    },
    mutations: {
        updatelisi(state.payload){
            state.name=payload;
        }
    },
    actions: {
        aupdatelisi(context) {
            //这里commit的是上面部分的mutations
            console.log(context);//有根的各种东西
            setTimeout(() => {
                context.commit('updatelisi', 'wangwu');
            }, 1000)
        }
    },
    getters: {
        fullname(state) {
            return state.name + ' getters'
        },
        fullname(state, getters ,rootState){
            //rootState 根的state
            return getters.fullname + rootState.counter;
        }
    }
}
const store = new Vuex.Store({
    ...
    modules: {
        a: moduleA
    }
})
//vue使用
<h3 style="color: red">{{ $store.state.a.name }}</h3>
<h3 style="color: red">{{ $store.getters.fullname }}</h3>
<h3 style="color: red">{{ $store.getters.fullname2 }}</h3>
<button @click="change">改名</button>
<button @click="asyncChange">异步改名</button>

methods:{
    change(){
        this.$store.commit('updatelisi',"张三");
    },
	asyncChange() {
		this.$store.dispatch("aupdatelisi");
	},
}
~~~



### 3.6 actions的写法 对象的解构

~~~js
actions:{
    add(context){
        ...
    },
    //也可以写成
    add({state,commit,rootState}){
       ...
    }
}
~~~



解构：

~~~js
const obj = {
    name:'11',
    age:18,
    height:1.88,
}

const {age , name} = obj;
~~~



## 4.store文件夹的目录组织

把mutations、actions、getters全部抽出文件 导入到`index.js`

建modules文件夹，放moduleA.js …文件 导入`index.js`

state一般不抽







