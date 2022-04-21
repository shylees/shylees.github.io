---
title: nuxtjs 开发 spa 遇上的有关 html 标签规则的报错
data: 2022/04/20 17:21
updata: 2022/04/20 21:41
tag:
  - ssr
  - nuxtjs
category:
  - workNotes
---

> 博客上线前，老大发现的 [vue warn], 说 “博客详情服务器渲染有问题”、“这个在正式环境会执行错误的”、“沙盒应该就会无法渲染”、“ssr 和 spa 渲染结果不一样”...
>
> 触发报错的操作: “在博客详情页直接刷新 就会提示我截图的错误”

### 报错信息:

[Vue warn]: The client-side rendered virtual DOM tree is not matching server-rendered content. This is likely caused by incorrect HTML markup, for example nesting block-level elements inside `<p>`, or missing `<tbody>`. Bailing hydration and performing full client-side render.

### 报错截图：

<img src="https://s1.ax1x.com/2022/04/20/Ls48OK.png">

> 大概意思是 HTML 结构嵌套不正确，会导致 srr 渲染的页面跟csr的不一样
> 根据黄色提示，我很快就定位到了代码的位置

### 报错效果图及其 html 标签:

<img src="https://s1.ax1x.com/2022/04/20/LsI3VO.jpg">

最外层的`<a class="pre item">`

> 本来最外层的 `<a>` 是 `<p>` 的，但是因为里面的两个 `<a>` 标签都是跳转同一个链接，所以我就直接把最外层改了，虽然我记得这样嵌套不行，但是我以为没影响！

> 我不知道为什么上个同学要分开写两个? 难道是因为分开 `hover` 改变样式? 如果有大佬知道分开写的好处, d 我!!! 我很感兴趣!!!

### 报错解决

因为 html 标签嵌套规则里面有提到 `<a>` 标签可以嵌套**除本身以外**的块级元素、行内元素，所以直接把里面的两个 `<a>` 改成其他标签，如 `<span>` 就解决问题啦！
