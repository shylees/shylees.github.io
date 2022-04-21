---
title: flex 布局
date: 2020/11/7
updated: 2020/11/7
comments:
tags:
 - css
categories:
 - learningNotes
layout:
permalink:
meta: true
---

#### flex 布局是什么

Flex 是 Flexible Box 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。

任何一个容器都可以指定为 Flex 布局。`display: flex;`

行内元素也可以使用 Flex 布局。`display: inline-flex;`

Webkit 内核的浏览器，必须加上`-webkit`前缀。`display: -webkit-flex; display: flex;`

> 注：设为 Flex 布局以后，子元素的`float`、`clear`和`vertical-align`属性将失效。

​	

#### 基本概念

采用 Flex 布局的元素，称为 **Flex 容器**（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为 **Flex 项目**（flex item），简称"项目"。



#### 容器的属性

- flex-direction：决定主轴的方向（即项目的排列方向）

  `row 默 | row-reverse | column | column-reverse;`

- flex-wrap：定义,一条轴线排不下，如何换行

  `nowrap 默 | wrap | wrap-reverse;`

- flex-flow：是`flex-direction`属性和`flex-wrap`属性的简写形式

- justify-content：定义了项目在主轴上的对齐方式

  `flex-start 默 | flex-end | center | space-between | space-around;`

- align-items：定义项目在交叉轴上如何对齐

  `flex-start  | flex-end | center | baseline | stretch 默 `

- align-content：定义了多根轴线的对齐方式，如果项目只有一根轴线，不起作用

  `flex-start | flex-end | center | space-between | space-around | stretch 默`



#### 项目的属性

- `order`：定义项目的排列顺序。数值越小，排列越靠前，默认为0

- `flex-grow`：项目的放大比例，默认为0，即如果存在剩余空间，也不放大

- `flex-shrink`：性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小

- `flex-basis`：在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为`auto`，即项目的本来大小。

  `<length> | auto `

- `flex`：`flex-grow`, `flex-shrink` 和 `flex-basis`的简写，默认值为`0 1 auto`。后两个属性可选

- `align-self`：允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性

  `auto | flex-start | flex-end | center | baseline | stretch`