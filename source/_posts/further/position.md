---
title: position
date: 2020/3/28
updated: 2020/3/28
comments:
tags:
 - css
categories:
 - learningNotes
layout:
permalink:
meta: true
---

css **position** 属性用于指定一个元素在文档中的定位方式。top、right、bottom、left 属性决定了该元素的最终位置。

#### 定位类型

+ `static`

  指定元素使用正常的布局行为，即元素在文档常规流中当前的布局位置。

  > 此时 `top`, `right`, `bottom`, `left` 和 `z-index `属性无效。

+ `relative`

  元素先放置在未添加定位时的位置，再在不改变页面布局的前提下调整元素位置（因此会在此元素未添加定位时所在位置留下空白）。

  > `position:relative ` 对 `table-*-group`, `table-row`, `table-column`, `table-cell`,` table-caption` 元素无效。

+ `absolute`

  元素会被移出正常文档流，并不为元素预留空间，通过指定元素相对于最近的非 static 定位祖先元素的偏移，来确定元素位置。

  > 绝对定位的元素可以设置外边距（margins），且不会与其他边距合并。

+ `fixed`

  元素会被移出正常文档流，并不为元素预留空间，而是通过指定元素相对于屏幕视口（viewport）的位置来指定元素位置。元素的位置在屏幕滚动时不会改变。打印时，元素会出现在的每页的固定位置。

  > `fixed` 属性会创建新的层叠上下文。当元素祖先的 `transform`, `perspective` 或 `filter` 属性非 `none` 时，容器由视口改为该祖先。

+ `stick`

  元素根据正常文档流进行定位，然后相对它的*最近滚动祖先（nearest scrolling ancestor）*和 [containing block](https://developer.mozilla.org/en-US/docs/Web/CSS/Containing_block) (最近块级祖先 nearest block-level ancestor)，包括table-related元素，基于`top`, `right`, `bottom`, 和 `left`的值进行偏移。偏移值不会影响任何其他元素的位置。

  > 须指定 `top`, `right`, `bottom`或`left`四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。



#### 定位的特殊性

  绝对定位和固定定位和浮动相似：

1. 行内元素添加绝对/固定定位，可直接设置宽高                                                                                      
2. 块级元素添加绝对/固定定位，if不给宽/高，默认大小是内容大小                                                                                    
3.  脱标（浮动/绝对/固定）的盒子不会触发外边距塌陷
