---
title: CSS3 animate
date: 2021/10/26
updated: 2021/10/26
comments:
tags:
 - css
categories:
 - learningNotes
layout:
permalink:
meta: true
---

## CSS3 animate

**CSS animations** 使得可以将从一个CSS样式配置转换到另一个CSS样式配置。动画包括两个部分:描述动画的样式规则和用于指定动画开始、结束以及中间点样式的关键帧。

#### 制作动画

1. 定义动画

   ~~~css
   @keyframs myanimate{
       0%{}
       100%{}
   }
   ~~~

2. 使用动画

   ~~~css
   div{
       ...
       animation:name duration timing-function delay iteration-count direction fill-mode;
     /*动画名 持续时间 运动曲线 何时开始 播放次数 是否反方向 动画起始或者结束的状态*/
       /* 简写不包括 play-state 运行还是暂停 */
   }
   ~~~



#### 常见属性

| 属性                      | 描述                                                        |
| ------------------------- | ----------------------------------------------------------- |
| @keyframes                | 规定动画                                                    |
| animation                 | 所有动画的简写属性，除了animation-play-state属性            |
| animation-name            | 规定@keyframes动画的名称（必须）                            |
| animation-duration        | 规定动画完成一个周期所花费的秒或毫秒，默认为0（必须）       |
| animation-timing-function | 规定动画是的速度曲线，默认为ease                            |
| animation-delay           | 规定动画合适开始，默认是0                                   |
| animation-iteration-count | 规定动画播放的次数，默认1，还有ifinite（无限）              |
| animation——direction      | 规定动画是否在下一周逆向播放，默认为normal，逆播放alternate |
| animation-play-state      | 规定动画是否正在运行或停止，默认running 还有paused          |
| animation-fill-mode       | 规定动画结束后状态，保持forwards回到起始backwards           |



#### 总结：

简写属性不包括animation-play-state
	          暂停动画：animation-play-state：puased；经常和鼠标经过等其他配合使用
	          想要动画走回来，而不是直接跳回来：animation-direction：alternate；
	          盒子动画结束后，停在结束位置：animation-fill-mode：forwards；

