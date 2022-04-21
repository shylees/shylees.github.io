---
title: 为什么 data 属性是一个函数而不是一个对象？
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

> 参考链接:
>
> https://vue3js.cn/interview/vue/data.html


### 1. 实例和组件定义 data 的区别

+ vue 实例的时候定义 data 属性可以是对象也可以是函数

  ~~~js
  const app = new Vue({
      el:"#app",
      data:{ // 对象
          foo:"foo" 
      },  
      data(){ //函数
          return { foo:"foo" } 
      } 
  })
  ~~~

+ 组件中定义 data 属性，只能是一个函数，如果直接定义为一个对象，会报警告

  警告说明：返回的 data 应该是一个函数在每一个组件实例中

### 2. 组件 data 定义函数与对象的区别

在定义好一个组件时， vue 最终会通过 vue.extend() 构成组件实例

当组件定义 data 属性时，采用对象的形式，在使用该组件创建多个组件实例的时候，当一个组件修改了data里面的值，其他组件的 data 也会被修改。

but 采用函数的形式时，就不会出现这种请求，因为函数返回的对象内存地址不相同，修改一个实例 data 时，其他组件实例 data 不受影响

### 3. 结论

+ 根实例对象 data 可以是对象也可以是函数，根实例是单例，不会产生数据污染的情况
+ 组件实例对象 data 必须是函数，目的是为了防止多个组件实例对象之间共用一个 data，产生数据污染。采用函数的形式，initData 时会将其作为工厂函数都会返回全新 data 对象