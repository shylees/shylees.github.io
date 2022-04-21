---
title: markdown语法/typora使用及其快捷键 
date: 2020/11/2
updated: 2020/11/2
comments:
tags:
categories:
 - learningNotes
layout:
permalink:
meta: true
---

超详细：https://blog.csdn.net/SIMBA1949/article/details/79001226

##### 标题

’#‘ 一阶标题 ctrl+1

‘##’ 二阶标题  ctrl+2

…

‘######’ 六阶标题 ctrl+6



##### 文本居中

'\<center\>这是要居中的文本\<center\>’
<center>居中</center>
<center>这是要居中的文本</center>

##### 下划线

<u>或者ctrl+u</u>

##### 删除线

~~或者alt+shift+5~~

##### 字体加粗

**或者ctrl+B 或者一对__ l**

##### 字体倾斜

*或者ctrl+i 或者 一对_ l*

##### 字体倾斜加粗

***或者用一对___ l*** 

##### 图片插入

可直接拉入

或者![]()

##### 超链接

<http://www.simba.com>  //<直接输入>

[百度百科](www.baidu.com) //快捷健 ctrl+k  or /[自定义内容 /]/(超链接地址)

* 打开本地文件时

  ```java
  [打开linkTest.md文档](./linkTest.md)
  ```

* 页内跳转

  ```java
  [链接文字](#标题文字)
  ```

  [标题](# 标题)

##### 代码行

一对`包住

`printf("hello")`

##### 代码段

三个`+enter /空格 +编程语言

```java
System.out.printf("hello");
```



##### 表格使用

ctrl+t 会自动跳出设置行和列的设置框 

使用 `|` 来分隔不同的单元格，使用 `-` 来分隔表头和其他行

or |国际|省市|市区|

| 国际 | 省市 | 市区 |
| :--- | ---- | ---- |
|      |      |      |



##### 任务列表

-/+/*无序列表

* 

+ 

- java

-[] 多选框（注意用空格隔开）

- [ ] java

任意数字+.+空格 有序列表

1. 

##### 数学表达式

行内：由一个美元符号将公式包起来  $cosx+sinx=1$

行外：ctrl+shift+m 
$$
cosx+sinx=1
$$

##### 上标下标

可以使用^,_后跟相应符号实现

$a^1$  $a_1$  $a^{32}$

##### 根号

可以用\sqrt{}  \sqrt[]{}

$\sqrt{5}$  $\sqrt[4]{10}$

##### 上下水平线

\underline{} \overline

$\underline{a+1}$ $\overline{a+1}$



#### 更多数学公式在：https://blog.csdn.net/SIMBA1949/article/details/79001226



##### 水平分割线

***或 - - -（不行）

***

##### 引用

‘>’+空格

>引用

##### 注释

[^ 注释内容]

##### 插入目录

可以在输入[toc]命令的地方自动根据标签生成目录

##### 导出

选择文件–>导出

##### 文本高亮

一对==

==高亮==

##### 表情

:单词

:dancer:

<img src="img\typora.png" style="zoom:65%; float:left;" />



#### typora与html

##### 改变文字颜色

用font标签改变大小及颜色

<font size=2 color="red">字体为red ，大小为2</font>

##### 改变对齐方式

<p align="left">左对齐</p>

<p align="center">中间对齐</p>

<p align="right">右对齐</p>

##### 插入图像

通过<img scr=url />

<img src="img/typora1.jpg" width=100 height=100 align=left>

