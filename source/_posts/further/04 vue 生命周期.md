---
title: vue生命周期
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

## vue 生命周期

+ beforeCreate：<u>data 和 methods 中的数据还没有初始化</u>
+ created：<u>data 和 methods 已经被初始化了</u>，**要调用 methods 或 操作 data 中的数据最早只能在这**
+ beforeMount：（模板在内存中编译好，还未挂载到页面上）<u>页面的元素还没被替换过来，还是之前的模板字符串</u>，[render 函数在这被调用，生成虚拟 DOM ]
+ mounted：<u>内存中的模板已经真实的挂载到页面中</u>，是实例创建期间的最后一个生命周期，执行完 mounted 就表示实例已经被完全创建好了，**要通过某些插件操作页面上的 DOM 节点，最早要在这**
+ beforeUpdate：组件运行阶段的生命周期，<u>页面中显示的数据还未更新，但data数据是最新的</u>，[数据更新后，新的虚拟 DOM 生成，但还没跟旧虚拟 DOM 对比补丁]
+ update：<u>页面和data已经保持同步了</u>，[新的虚拟 DOM 对比补丁后，进行真实 DOM 的更新]
+ beforeDestory：vue实例进入销毁阶段，实例上所有的 data methods 过滤器 指令<u>都处于可用状态</u>，此时还未真正执行销毁
+ destoryed：组件已经被完全销毁，此时组件中所有的 data methods… 都<u>已经不可用了</u>

 

+ activated：keep-alive 专属，组件被激活时调用
+ deactivated：keep-alive 专属，组件被销毁时调用



**Q：异步请求在哪里发起？**

可以在钩子函数 created、beforeMount、mounted 中进行异步请求，因为这三个函数中，**data 已经创建，可以将服务器端返回的数据进行赋值**

如果异步请求不需要依赖 dom 推荐在 created 函数中调用异步请求，优点有：

+ 能更快获取到服务器数据，减少页面 loading 时间
+ ssr 不支持 beforeMount、mounted 钩子函数，所以放在 created 中有助于一致性

