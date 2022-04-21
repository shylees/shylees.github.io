---
title: BFC 和 触发BFC
date: 2021/10/26
updated: 2021/10/26
comments:
tags:
 - css
categories:
 - furtherNotes
layout:
permalink:
meta: true
---


#### 1.常见定位方案

- 普通流 (normal flow)

> 在普通流中，元素按照其在 HTML 中的先后位置至上而下布局，在这个过程中，行内元素水平排列，直到当行被占满然后换行，块级元素则会被渲染为完整的一个新行，除非另外指定，否则所有元素默认都是普通流定位，也可以说，普通流中元素的位置由该元素在 HTML 文档中的位置决定。

- 浮动 (float)

> 在浮动布局中，元素首先按照普通流的位置出现，然后根据浮动的方向尽可能的向左边或右边偏移，其效果与印刷排版中的文本环绕相似。

- 绝对定位 (absolute positioning)

> 在绝对定位布局中，元素会整体脱离普通流，因此绝对定位元素不会对其兄弟元素造成影响，而元素具体的位置由绝对定位的坐标决定。



#### 2.BFC是什么

**块格式化上下文（Block Formatting Context，BFC）** 是Web页面的可视CSS渲染的一部分，是块盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域。属于上述定位方案的普通流。

> **具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。**
>
> 通俗一点来讲，可以把 BFC 理解为一个封闭的大箱子，箱子内部的元素无论如何翻江倒海，都不会影响到外部。



#### 3.触发/创建 BFC

简洁版：

+ body根元素
+ 浮动元素：float 除 none 以外的值
+ 绝对定位元素：position (absolute、fixed)
+ display ( inline-block、table-cells、flex )
+ overflow 除 visible 以外的值 ( hidden、auto、scroll )



详细版：

> - 根元素（`<html>`）
> - 浮动元素（元素的 `float` 不是 `none`）
> - 绝对定位元素（元素的 `position` 为 `absolute` 或 `fixed`）
> - 行内块元素（元素的 `display`为 `inline-block`）
> - 表格单元格（元素的 `display`为 `table-cell`，HTML表格单元格默认为该值）
> - 表格标题（元素的 `display`为 `table-caption`，HTML表格标题默认为该值）
> - 匿名表格单元格元素（元素的 `display`为 `table`、`table-row`、 `table-row-group`、`table-header-group`、`table-footer-group`（分别是HTML table、row、tbody、thead、tfoot 的默认属性）或 `inline-table`）
> - `overflow` 计算值(Computed)不为 `visible` 的块元素
> - `display`值为 `flow-root` 的元素
> - `contain` 值为 `layout`、`content `或 `paint` 的元素
> - 弹性元素（`display` 为 `flex` 或 `inline-flex `元素的直接子元素）
> - 网格元素（`display`为 `grid` 或 `inline-grid` 元素的直接子元素）
> - 多列容器（元素的 `column-count` 或 `column-width` (en-US)不为 `auto，包括 ``column-count` 为 `1`）
> - `column-span` 为 `all` 的元素始终会创建一个新的BFC，即使该元素没有包裹在一个多列容器中（标准变更，Chrome bug）。
>
> 块格式化上下文包含创建它的元素内部的所有内容.





#### 4.BFC 特性及其应用

+ 同一个  BFC 下外边距会发生折叠

  > 解决外边距塌陷：将其放在不同的 BFC 容器中

+ BFC 可以包含浮动的元素

  > 清除浮动：父元素触发 BFC

+ BFC 可以阻止元素被浮动元素覆盖

