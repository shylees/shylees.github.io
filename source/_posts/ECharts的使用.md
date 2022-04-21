---
title: EChart的使用
date: 2021/7/1
updated: 2021/8/17
comments:
tags:
 - js组件
categories:
 - learningNotes
layout:
permalink:
meta: true
---

## 1. EChart的使用(2021/07/01 pink老师)

### 1.1 使用步骤

1. 下载并导入echarts.js文件 (@4.9.0 才有地图)

   > 有多种导入方式 使用直接下载文件然后导入就可以了[下载地址](https://echarts.apache.org/zh/download.html)

2. 准备一个有宽高的DOM容器

3. 初始化echarts实例

   > const mychart = echarts.init(document.querySelector('.box'));

4. 指定配置项和数据option

   >  let option = { … }

5. 将配置项设置给echarts实例对象

   > mychart.setOption(option);



### 1.2 基础配置 

> option里面的配置

+ `title`: 设置图表标题

+ `tooltip`: 提示框组件
  + `trigger`: 触发方式
    + 值为`axis`时鼠标在坐标轴上时触发
  + `axisPointer`： 坐标轴指示器
    + 值默认为`line` 也可以设置成`shadow`
+ `legend`：图例组件
  + `textStyle`: 图例文字的样式
    + `color`
  + `left、right、top、bottom`: 图例位置
+ `toolbox`： 工具箱组件 可以保存图片什么的
+ `color`： 后面的值是数组形式 线条的颜色

> 直角坐标系的配置

+ `grid`: 网格 – 控制线型图 柱状图 图表的大小

  + `left、right、top、bottom`： 控制大小
  + `containLabel`: 显示刻度标签

+ `xAxis`： 设置x轴的配置

  + `type`： `category` 类目、`value`值
  + `boundaryGap`:线条与坐标轴是否有缝隙
  + `data`：x轴的相关显示信息
  + `axisLabel`: 刻度标签的相关样式 有文字颜色 大小 样式什么的
  + `axisLine`： 坐标轴那条线的样式
    + `show`：表是否显示

+ `yAxis`: y轴的配置

  + `axisLine`： 坐标轴那条线的样式 if要设置样式

    + `lineStyle`: 在里面配置样式

  + `axisTick`: 坐标轴的刻度

    + `show` : 是否显示

  + `splitLine`: y轴分割线的样式

    + `lineStyle`:

    

+ `series`: 系列图表的配置 决定显示哪种类型的图表

  + `stack`: if值相同 会发生数据堆叠

  + `name`: 与上legend的数组相同 if有name可不用legend

  + `barWidth`: 柱子的宽度

  + `barCategoryGap`: 柱子距离

  + `label`: 显示柱子内的文字

    + `show` : 是否显示
    + `position` : `“inside”`
    + `formatter` : `“{c}%” `  //c 数据值 b 数据名 a系列名 会自动解析为data里的数据

  + `itemStyle`: 每个柱子的样式

    + `barBorderRadius`：圆角边框

    + `color`：

      >一般来说只能有一个颜色，就算是数组形式 也只会展示最后一个颜色
      >
      >如果想要展示多个的话 可以
      >
      >先
      >
      >~~~js
      >const mycolor = ["pink", "blue", "yellow", "white", "black"]
      >//然后在里面
      >color:function(params){
      >// params传进来的是柱子对象    
      >    return mycolor[params.dataIndex]
      >}
      >~~~
      >
      >
      >
      >如果想要展示一个框
      >
      >~~~js
      >color:"none",
      >borderColor:"pink",
      >~~~
      >
      >

  

  <img src='https://s1.328888.xyz/2022/04/09/XkQud.jpg'>



### 1.3 柱形图

##### 更改对应数据

横坐标的数据：`axis` 的`data`

纵坐标的数据: `series` 的`data`

使图表跟屏幕大小做自适应

~~~js
window.addEventListener("resize",function(){
    myChary.resize();
})
~~~

#####  实现两组柱子层叠

在series里的两个柱子对象里

使用`yAxisIndex:0`和`yAxisIndex:1` 在不同的柱子上

##### 坐标轴数组反转

在yAxis里`inverse:true`



### 1.4 折线图（2021/7/6）

##### 改线的颜色

option里直接加`color:[“”]`

##### 将折线改圆滑

在series里要平滑显示的线条对象 加上`smooth:true`

##### 单独改变线的样式

在series里 `lineStyle:{...}`

##### 渐变色填充区域

在series里

~~~js
areaStyle:{
    // 渐变色，只需要复制即可
    color:new echarts.graphic.LinearGradient(0,0,0,1,
          [{
              offset:0,
              color:"rgba(1,132,213,0.4)"  //起使颜色
          },{
              offset:0.8,
              color:"rgba(1,132,213,0.1)"  //结束颜色
          }],false),
        shadowColor:"rgba(0,0,0,0.1)"
},
~~~

##### 拐点

在series里

`symbol:"circle"`

`symbolSize:12`

`showSymbol:false` 一开始不显示，鼠标经过才显示

`itemStyle:{color:"" , borderColor:"" , borderWidth: ""}`拐点的样式



### 1.5 饼形图

##### 触发方式

`tooltip:{ trigger: 'item', ...}`

鼠标悬停饼

##### 鼠标悬停部分饼状 中间出现文字

~~~js
series:[
    { 
        ... , 
        emphasis:{ 
        	label:{
        		show:true,
        		fontSize：’30‘,
        		...
    		}
    	},
    	...
    } 
]
~~~

##### 修改图例

~~~js
legend:{
    bottom:"5%", //图例位置
    itemWidth/itemHeight:10, //图标大小
    textStyle:{} //修改图例文字
}
~~~

##### 修改在容器中的位置

`series:[ { center:[ "50%" , "50%" ] } ]`

##### 修改饼形大小

`series:[ { radius:[ "40%" , "50%" ] } ]`

内圆半径，外圆半径

##### 模式

`series:[ { roseType:"radius/area" } ] //半径模式 / 面积模式`

##### 图形的文字 和 线

~~~js
series:[ 
    { 
    	label:{
          	fontSize:10  
        },
        labelLine:{
        	length:50 //链接图形的线
        	length2:10  //链接文字的线条
    	}
    } 
]
~~~



### 1.6 EChart社区 

### 1.7 地图

先引入地图js

##### 改变鼠标悬停后的颜色

~~~js
geo:{
    normal:{
        areaColor:"pink",
        borderColor:"..."
        borderWidth:1
    }
}
~~~

##### 改变大小

`geo:{ zoom: 1.2 }`











