---
title: html 标签嵌套规则
data: 2022/4/20 22:23
updata: 2022/4/23 16:30
tag:
  - html
category:
  - workNotes
---

> 由 [nuxtjs 开发 spa](./10 nuxtjs 报错.md) 引申出的思考
>
> 参考链接：
> https://cloud.tencent.com/developer/article/1009200
>
> https://cloud.tencent.com/developer/article/1484900
>
> https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/Content_categories

> 其实第一个参考链接的图片已经讲的很清楚了，那我就大概总结一下吧

1. 块级元素可以嵌套 块级/行内元素

   - 其中 `<h1>～<h6>` `<dt>` 只能嵌套行内元素
   - 其中 `<p>` 可以嵌套 `<p>` 和 行内元素

2. 行内元素 只可以嵌套行内元素

   - 其中 `<a>` 可以嵌套除了 `<a>` 以外的几乎所有元素，包括行内和块级元素

3. 块级元素 与 块级元素 并列，行内元素 与 行内元素 并列

> 在使用 nuxtjs 配合 vue 开发的时候，报 `[vue warn]`的情况，只有 `<a> <a></a> </a>`，及 a 标签 嵌套 a 标签才会出现，为啥捏？还没有搞懂...
