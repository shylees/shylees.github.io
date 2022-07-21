---
title: v-if、v-for、v-show
date: 2021/12/10
updated: 2021/12/10
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
> https://vue3js.cn/interview/vue/show_if.html#%E4%B8%80%E3%80%81v-show%E4%B8%8Ev-if%E7%9A%84%E5%85%B1%E5%90%8C%E7%82%B9

> https://juejin.cn/post/6984210440276410399#heading-18
>
> https://vue3js.cn/interview/vue/if_for.html#%E4%BA%8C%E3%80%81%E4%BC%98%E5%85%88%E7%BA%A7

## v-if 与 v-show 的区别、v-if 为什么不建议与 v-for 一起使用

### 1. v-show 与 v-if 的区别

+ 控制手段：

  + v-show：显示隐藏式为该元素**添加 display 属性**，dom 元素一直存在
  + v-if：显示隐藏式将 **dom 元素整个添加或删除**

+ 编译条件：

  + v-show：会**被编译成指令**，条件不满足时控制样式将对应节点隐藏

  + v-if：会**被转换成三元表达式**，条件不满足时不渲染

    > 是真正的条件渲染，它会确保在切换过程中，条件块内的事件监听器和子组件销毁和重建，只有渲染条件为假时，不做操作，

+ 编译过程：

  + v-show：简单的**基于 css 切换**
  + v-if：有一个**局部编译 / 卸载过程**，切换过程中适当地销毁和重建内部的事件监听和子组件

+ 状态切换：

  + v-show：**不会触发生命周期**

  + v-if：false → true 触发组件的 beforeCreate、created、beforeMount、mounted；

    ​          true → false 触发组件 beforeDestory、destoryed

+ 性能消耗：

  +  v-show：有更高的**初始化渲染消耗**
  + v-if：有更高的**切换消耗**

### 2. v-if 不建议与 v-for 一起使用

v-if 和 v-for 都是 vue 模板系统中的指令，在 vue 模板编译的时候，会将指令系统转化成可执行的 render 函数。

**v-for 比 v-if 的优先级高**，当 v-if 和 v-for 在同一个标签时，会先遍历渲染，然后再进行判断是否展示，所以就会渲染一些无用节点，增加无用的 dom 操作，可以**通过 computed 或者 \<template\> 解决**

> eg：
>
> ```html
> <div v-for="item in [1, 2, 3, 4, 5, 6, 7]" v-if="item !== 3">
>     {{item}}
> </div>
> ```
>
> 上面的写法是`v-for`和`v-if`同时存在，会先把7个元素都遍历出来，然后再一个个判断是否为3，并把3给隐藏掉，这样的坏处就是，**渲染了无用的3节点，增加无用的dom操作，建议使用computed来解决**这个问题：
>
> ```vue
> <div v-for="item in list">
>     {{item}}
> </div>
> <script>
> computed() {
>     list() {
>         return [1, 2, 3, 4, 5, 6, 7].filter(item => item !== 3)
>     }
>   }
> </script>
> ```



所以有这样的注意事项：

1. 不要把 v-if v-for 放在同一个元素上，会带来性能方面的浪费

2. 如果避免出现浪费又非得再同一个标签上，可以在**外层嵌套 template 标签**，**页面渲染不会生成 dom 节点**，在这层进行 v-if 判断，然后在内部进行 v-for 循环

   > ~~~html
   > <template v-if="isShow">
   >     <p v-for="item in items">
   > </template>
   > ~~~

3. 如果条件出现在**循环内部**，可以**通过计算属性 computed 提前过滤**不需要显示的项

   > ~~~js
   > computed: {
   >     items: function() {
   >       return this.list.filter(
   >           function (item) {
   >         		return item.isShow
   >           }
   >       })
   >     }
   > }
   > ~~~

