---
title: nuxtjs 的生命周期
date: 2022/2/21 11:28
updated: 2022/2/21 11:35
comments:
tags:
 - ssr
categories:
 - workNotes
layout:
permalink:
meta: true
---

## asyncData
> 可能想要在服务器端获取并渲染数据。Nuxt.js添加了asyncData方法使得你能够在渲染组件之前异步获取数据。

asyncData 是最常用最重要的生命周期，同时也是服务端渲染的关键点。
该生命周期只限于页面组件调用，第一个参数为 context。
它调用的时机在组件初始化之前，运作在服务端环境。
所以在 asyncData 生命周期中，我们**无法通过 this 引用当前的 Vue 实例**，也**没有 window 对象和 document 对象**，这些是我们需要注意的。
一般在 asyncData 会对主要**页面数据进行预先请求**，获取到的数据会交由服务端拼接成 html 返回前端渲染，以此提高首屏加载速度和进行 seo 优化。


## fecth
> 用于在渲染页面前填充应用的状态树（store）数据， 与 asyncData 方法类似，不同的是它**不会设置组件的数据。**

得知该生命周期用于填充 Vuex 状态树，与 asyncData 同样，它在组件初始化前调用，第一个参数为 context。但这并不是说我们只能在 fetch 中填充状态树，在 asyncData 中同样可以。

## validate
> 但这并不是说我们只能在 fetch 中填充状态树，在 asyncData 中同样可以。

在验证路由参数合法性时，它能够帮助我们，第一个参数为 context。与上面有点不同的是，我们能够访问实例上的方法 this.methods.xxx。


## watchQuery
> 监听参数字符串更改并在更改时执行组件方法 (asyncData, fetch, validate, layout, ...)

watchQuery 可设置 Boolean 或 Array (默认: [])。使用 watchQuery 属性可以监听参数字符串的更改。 如果定义的字符串发生变化，将调用所有组件方法(asyncData, fetch, validate, layout, ...)。 为了提高性能，默认情况下禁用。
使用 watchQuery有点好处就是，当我们使用浏览器后退按钮或前进按钮时，页面数据会刷新，因为参数字符串发生了变化。

## head
> Nuxt.js 使用了 vue-meta 更新应用的 头部标签(Head) 和 html 属性。

使用 head 方法设置当前页面的头部标签，该方法里能通过 this 获取组件的数据。
除了好看以外，正确的设置 meta 标签，还能有利于页面被搜索引擎查找，进行 seo 优化。
一般都会设置 description(简介) 和 keyword(关键词)。


## 生命周期的调用顺序

validate  =>  asyncData  =>  fetch  =>  head

> 参考链接 https://juejin.cn/post/6844904160324747278#heading-12