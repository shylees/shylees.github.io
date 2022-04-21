---
title: vue组件通信的方式
date: 2021/12/7
updated: 2021/12/7
comments:
tags:
 - vue
categories:
 - furtherNotes
layout:
permalink:
meta: true
---

> 参考链接：
>
> https://juejin.cn/post/6844903887162310669  8
>
> https://juejin.cn/post/6999687348120190983  12
>
> https://vue3js.cn/interview/vue/communication.html 8


## 总结

+ 父子之间的通信：`props` ; `$parent / $children`；`provide / inject`；`ref` ；`$attrs / $listeners`
+ 兄弟组件通信：`eventBus`；`vuex`
+ 跨级通信：`eventBus`；`Vuex`；`provide / inject`；`$attrs / $listenters`

## 组件通信有哪几种方式

#### 1.props / $emit：

适用场景：父组件传递数据给子组件 / 子组件传递数据给父组件

父组件向子组件传递数据是通过 props 传递的，子组件传递数据给父组件是通过 \$emit 触发事件 做到的



#### 2. $children / \$parent：

指定已创建的实例之父实例，在两者之间建立父子关系。子实例可以用 this.$parent 访问父实例，子实例被推入父实例的 \$children 数组中。

> \$parent 是对象；\$children 是数组
>
> 其目的主要是作为访问组件的应急方法，更推荐 props 和 event 实现父子通信



#### 3. provide / inject：

父组件通过 provide 来提供变量，然后在子组件中通过 inject 来注入变量

> ~~~vue
> // A.vue
> <div> <comB></comB> </div>
> <script>
> export default{
>     name:"A",
>     provide:{ for: "demo" }
> }
> </script>
> 
> // B.vue
> <div> {{ demo }} <comC></comC> </div>
> <script>
> export default{
>     name:"B",
>     inject:['for'],
>     data(){ return { demo:this.for } }
> }
> </script>
> 
> // C.vue
> <div> {{ demo }} </div>
> <script>
> export default{
>     name:"C",
>     inject:['for'],
>     data(){ return { demo:this.for } }
> }
> </script>
> ~~~



#### 4. ref / $refs

适用场景：父组件在使用子组件时设置 ref、父组件通过设置在子组件 refs 获取数据

ref 如果在普通的 dom 元素使用，引用指向的就是 dom 元素；

如果用在子组件上，引用就指向组件实例，可以**通过实例直接调用组件的方法或访问数据**

> ~~~vue
> // app.vue
> <div> <comA ref="comA"></comA> </div>
> <script>
> export default{
>     mounted(){
>         const comA = this.$refs.comA;
>         const nameA = comA.name;
>     }
> }
> </script>
> 
> // comA.vue
> <script>
> export default{
>     data(){
>         return {
>             name:'A'
>         }
>     }
> }
> </script>
> ~~~



#### 5. **eventBus**

使用场景：兄弟组件传值

事件总线，在 vue 中可以使用其作为沟通桥梁的概念，就像是所有组件共用相同的事件中心，可以向该中心注册发送事件或接收事件，所有组件都可以通知其他组件。当项目比较大的时候，容易造成难以维护的灾难。

> ~~~js
> // 1. 初始化
> // event-bus.js
> import Vue from 'vue'
> export const EventBus = new Vue()
> 
> // 2. 发送事件
> // showNumCom 和 additionNumCom 是兄弟组件 其实是父子也可以
> // additionNum.vue
> <div>  <button @click="additionHandle" >+</button>  </div>
> import {EventBus} from './event-bus.js'
> export default {
>     data(){
>         return { num:1 }
>     },
>     methods:{
>         additionHandle(){
>             EventBus.$emit('addition',{ num: this.num++ })
>         }
>     }
> }
> 
> // 3. 接收事件
> // showNum.vue
> <div> 计算和:{{count}} </div>
> import {EventBus} from './event-bus.js'
> export default {
>     data(){
>         return { count:0 }
>     },
>     mounted(){
>         EventBus.$on('addition',param => {
>             this.count = this.count + param.num;
>         })
>     }
> }
> // 实现了在组件 additionNum 中点击 + ，在 showNum 中利用传递来的 num 展示求和的结果
> 
> // 4. 溢出事件监听者
> import {eventBus} from 'event-bus.js'
> EventBus.$off('addition',{})
> ~~~



#### 6. Vuex

vuex 是一个专门为 vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组建的 状态，并以相应的规则保证状态以一种可预测的方式发生变化。vuex 解决了 **多个视图依赖于同一状态** 和 **来自不同视图的行为需要变更同一状态** 的问题，讲开发者的精力聚焦于数据的更新而不是数据在组件之间的传递上

+ state：用于数据的存储，是 store 中的唯一数据源
+ getters：如 computed，基于 state 数据的二次包装，常用于数据的筛选和多个数据的相关性计算
+ mutations：类似函数，改变 state 数据的唯一途径，且不能用于处理异步事件
+ actions：类似于 mutation，用于提交 mutation 来改变状态，而不直接变更状态，可以包含任意异步操作
+ modules：类似命名空间，用于项目中将各个模块的状态分开定义和操作

#### 7. localStorage / seesionStorage

#### 8.  \$attrs / \$listeners

使用场景：祖先传递数据给子孙

+ $attrs：包含父作用域里除 class 和 style 除外的非 props **属性集合**。通过 this.\$attrs 获取父作用域中所有符合条件的属性集合，若还要继续传给子组件内部的其他组件，可以通过 v-bind=‘\$attrs’
+ $listeners：包含父作用域里 .native 除外的监听**事件集合**。如果还要继续传给子组件内部的其他组件，可以通过 v-on=“\$linteners”
+ **inheritAttrs**

> ~~~vue
> // parent.vue
> <child name="name" title='111'></child>
> 
> // child.vue
> <sun-child v-bind="$attrs"></sun-child>
> <script>
> export defalut{
> 	props:['name'], //这里可以接收也可不接收，if接收，this.$arrts 中就会少一个值
> 	mounted(){
> 		console.log(this.$attrs); // {title:111} , 
>                                   // if 上无接收 {name:'name',...}
> 	},
>     inheritAttrs: false, // 可以关闭自动挂载到组件根元素上的没有在props声明的属性
> }
> </script>
> ~~~
>
> 















