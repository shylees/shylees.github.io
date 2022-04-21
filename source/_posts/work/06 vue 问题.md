---
title: vue 一些注意的问题
date: 2022/2/21 17:40
updated: 2022/2/21 19:04
comments:
tags:
 - vue
categories:
 - workNotes
layout:
permalink:
meta: true
---

# vue 一些注意的问题
> 参考 官方文档

1. v-if 与 v-show
    v-if 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

    v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

    相比之下，v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

    一般来说，**v-if 有更高的切换开销，而 v-show 有更高的初始渲染开**销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。

2. 避免 v-if 与 v-for 一起使用
    v-for 优先级比 v-if 高，多了不必要的开销

3. v-for 中的 in 可以被 of 替代，它更接近 JavaScript 迭代器的语法； 
    v-for 中的参数依次为(item, keyname, value);
    不要使用对象或数组之类的非基本类型值作为 v-for 的 key。请用字符串或数值类型的值。

4. 数组更新检测
    Vue 将被侦听的数组的变更方法进行了包裹，所以它们也将会触发视图更新。这些被包裹过的方法包括：
    `push() pop() shift() unshift() splice() sort() reverse()`

5. @click="handle($event)" 在内联语句处理器中访问原始的 DOM 事件

6. v-model 
~~~html
<input v-model="searchText">
<!-- 等价于 -->

<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>
~~~
    v-model 在内部为不同的输入元素使用不同的 property 并抛出不同的事件：
    + text 和 textarea 元素使用 value property 和 input 事件；
    + checkbox 和 radio 使用 checked property 和 change 事件；
    + select 字段将 value 作为 prop 并将 change 作为事件。

7. 一个组件的data必须是 函数
    一个组件的 data 选项必须是一个函数，因此每个实例可以维护一份被返回对象的独立的拷贝

8. 注册组件
~~~js
// 全局注册
Vue.component('component-a', { /* ... */ })
Vue.component('component-b', { /* ... */ })
Vue.component('component-c', { /* ... */ })

new Vue({ el: '#app' })

// 局部注册

var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }

new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
~~~

9. prop 的写法
    在js中:`props:[postTitle]`;
    在html中:`<blog-post post-title="hello!"></blog-post>`

10. prop 可以通过 v-bind 动态赋值
~~~html
<blog-post v-bind:title="post.title"></blog-post>
~~~

11. 单项数据流
    所有的 prop 都使得其父子 prop 之间形成了一个单向下行绑定：父级 prop 的更新会向下流动到子组件中，但是反过来则不行。这样会防止从子组件意外变更父级组件的状态，从而导致你的应用的数据流向难以理解。

    额外的，每次父级组件发生变更时，子组件中所有的 prop 都将会刷新为最新的值。这意味着你不应该在一个子组件内部改变 prop。如果你这样做了，Vue 会在浏览器的控制台中发出警告。