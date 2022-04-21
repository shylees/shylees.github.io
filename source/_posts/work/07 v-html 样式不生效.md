---
title: 关于 v-html 内样式不生效
date: 2022/2/24 11:53
updated: 2022/2/25 13:53
comments:
tags:
 - vue
categories:
 - workNotes
layout:
permalink:
meta: true
---

> free-vpn 读取$t 里面的内容 作为 html 其中有个 a 标签有其他样式
> 参考链接 https://cloud.tencent.com/developer/article/1485232

关于 v-html

> 在 vue 使用中，指令 v-html 渲染页面经常用到，类似于 jQuery 的$('x').html( )去渲染。通过指令 v-html 渲染出来的内容还会带有原来的标签及其样式，如果需要修改或者重设其样式，应该如何去做呢？

- 采坑
     首先，我在 style 中用子级选择器去选中并修改样式，经过猛如虎的操作后，并没生效。F12 打开 Elements 调试，发现在 style 里面样式根本没加载上去，没有 class 中也没有类名出现。除此之外，渲染非该指令元素时，所有的类名会跟有 [data-v-xxxxxx]的东西。

- 排坑
  - 去掉 style 中的 scoped;
  - watch 监测数据变化;
  - 深度选择器 >>>

1.  在 vue 组件中，我们写 style 时，为了防止页面样式冲突，在每个组件中会加上 scoped 属性。经测试，去掉该属性即可渲染样式成功。但是在组件过多或者项目中大时，经常会出现页面样式冲突，因此该方法不建议使用。
2.  在 script>exportdefault 中,watch 属性可监听 v-html 所绑定值的变化。如果是后台请求的数据，那么可以在 watch 中监听改数据变化，当数据发生改变驱动视图后，动态绑定一个 class 来改变子级元素样式。此方法有一定局限性。
3.  深度选择器 >>>，可深度改变子级样式

```html
<style scoped>
  .test >>> * {
    width: 100%;
  }
</style>

<!-- 如果使用 scss或者 less等css扩展语言，则用 /deep/替代 -->

<style scoped type="text/scss" lang="scss">
  .test {
    /deep/ * {
      width: 100%;
    }
  }
</style>
```

scoped 属性导致 css 仅对当前组件生效，而 html 绑定渲染出的内容可以理解为是子组件的内容，一般情况下子组件不会被加上对应的属性，所以不会应用带有 scoped 的 css。
