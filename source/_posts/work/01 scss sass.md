---
title: sass -- css 预处理器
date: 2022/2/18 14:58
updated: 2022/2/18 16:39
comments:
tags:
 - css
categories:
 - workNotes
layout:
permalink:
meta: true
---

> 2022.02.18 在公司熟悉环境时看到用的 cass 所以先去了解
> 
> 参考网站 https://www.sass.hk/guide/
> 
> 介绍：https://juejin.cn/post/6844904169313140749
> 
> 简洁区别：https://juejin.cn/post/6844904086676963341

# sass css 预处理器

CSS 预处理器定义了一种新的语言，其基本思想是，用一种专门的编程语言，为 CSS 增加了一些编程的特性，将 CSS 作为目标生成文件，然后开发者就只要使用这种语言进行CSS的编码工作。

## 0.1 使用 预处理器的原因
CSS仅仅是一个标记语言，不可以自定义变量，不可以引用。
**CSS有具体以下几个缺点：**
+ 语法不够强大，比如无法嵌套书写，导致模块化开发中需要书写很多重复的选择器；
+ 没有变量和合理的样式复用机制，使得逻辑上相关的属性值必须以字面量的形式重复输出，导致难以维护。
+ 预编译很容易造成后代选择器的滥用

**使用预处理器的优点:**
+ 提供CSS层缺失的样式层复用机制
+ 减少冗余代码
+ 提高样式代码的可维护性

Sass 和 Less 这类语言，其实可以理解成 **CSS 的超集**，它们在CSS 原本的语法格式基础上，增加了编程语言的特性，如变量的使用、逻辑语句的支持、函数等。让 CSS 代码更容易维护和复用。

但浏览器最终肯定是只认识 CSS 文件的，它无法处理 CSS 中的那些变量、逻辑语句，**所以需要有一个编译的过程，将 Sass 或 Less 写的代码转换成标准的 CSS 代码，这个过程就称为 CSS 预处理。**


## 0.2 less & sass

+ Less （Leaner Style Sheets 的缩写） 是一门向后兼容的 CSS 扩展语言。因为 Less 和 CSS 非常像，**Less 仅对 CSS 语言增加了少许方便的扩展，学习很容易** .

+ sass，作为”世界上最成熟、最稳定、最强大的专业级CSS扩展语言”。**兼容所有版本的css，且有无数框架使用sass构建**，如Compass，Bourbon，和Susy。

> SASS版本3.0之前的后缀名为.sass，而版本3.0之后的后缀名.scss。

**相同之处：**
1、混入(Mixins)—class中的class；
2、参数混入—可以传递参数的class，就像函数一样；
3、嵌套规则—Class中嵌套class，从而减少重复的代码；
4、运算—CSS中用上数学；
5、颜色功能—可以编辑颜色；
6、名字空间(namespace)—分组样式，从而可以被调用；
7、作用域—局部修改样式；
8、JavaScript 赋值—在CSS中使用JavaScript表达式赋值。

**不同之处：**

> sass less

+ 环境: dart或其他  |  基于javascript,可以运行在 Node 或浏览器
+ 使用: 复杂  |  简单(相对而言)
+ 功能: 复杂  |  简单(相对而言)
+ 处理机制: 服务端处理  |  可以运行在 Node 或浏览器端
变量: 以 $ 开头  |  以 @ 开头
文件后缀: .sass 或. scss  |  .less

+ 在Less中，**仅允许循环数值**。
在Sass中，我们可以**遍历任何类型的数据**。但在Less中，我们只能使用递归函数循环数值。

+ 条件语句
Less 中并**不支持**条件语句，当然，可以通过内置函数 if 以及 and，or，not 这些来模拟条件语句。
在 Sass 中是**支持**条件语句的，但也不是像其他编程语言直接 if 这样通过保留字来编写，需要加个 @ 符号


## 1. 使用变量

sass 使用 `$` 作为标识变量
> 老版的 scss 使用 `!` 作为标识，改变的原因 -- 太丑了 ?

1. 声明 引用
    ~~~css
    $highlight-color: #F90;
    .selected {
        border: $highlight-border;
    }
2. 变量名用中划线 / 下划线
    随便、爱用啥用啥，且 sass 将其视为一样的，都指向一个变量


## 2. 嵌套 css 规则
~~~css 
    #content {
        article {
            h1 { color: #333 }
            p { margin-bottom: 1.4em }
        }
        aside { background-color: #EEE }
}

~~~
### 1.1 父选择器标识符 `&`
~~~css
        article a {
            color: blue;
            &:hover { color: red }
        }
        /*编译后*/
        article a { color: blue }
        article a:hover { color: red }
~~~

### 1.2 群组选择器 `,`
~~~css
.container {
  h1, h2, h3 {margin-bottom: .8em}
}
~~~

### 1.3 子组合选择器和同层组合选择器 `>` `+` `~`
~~~css
/* 子 */
article > section { border: 1px solid #ccc }

/* 同层相邻组合 */
header + p { font-size: 1.1em }

/* 同层全体组合 */
article ~ article { border-top: 1px dashed #ccc }
~~~

### 1.4 嵌套属性
~~~css
nav {
  border: {
  style: solid;
  width: 1px;
  color: #ccc;
  }
}

nav {
  border: 1px solid #ccc {
  left: 0px;
  right: 0px;
  }
}
~~~

## 3. 导入 sass 文件

+ css 中不常用的特性：`@import` 规则，它允许在一个`css` 文件中导入其他 `css` 文件。然而，后果是**只有执行到 `@import` 时，浏览器才会去下载其他 `css` 文件，这导致页面加载起来特别慢。**

+ sass也有一个@import规则，但不同的是，sass的@import规则在生成css文件时就把相关文件导入进来。这意味着所有相关的样式被归纳到了同一个css文件中，而无需发起额外的下载请求。

### 3.1 局部文件 / 部分文件
当通过@import把sass样式分散到多个文件时，你通常只想生成少数几个css文件。那些专门为@import命令而编写的sass文件，并不需要生成对应的独立css文件，这样的sass文件称为**局部文件**。

此约定即，**sass局部文件的文件名以下划线开头**。这样，sass就不会在编译时单独编译这个文件输出css，而只把这个文件用作导入。当你@import一个局部文件时，还可以不写文件的全名，即省略文件名开头的下划线。


### 3.2 默认变量值 `!default`
~~~css
$fancybox-width: 400px !default;
.fancybox {
    width: $fancybox-width;
}
~~~
> 如果用户在导入你的sass局部文件之前声明了一个`$fancybox-width`变量，那么你的局部文件中对`$fancybox-width`赋值`400px`的操作就无效。如果用户没有做这样的声明，则`$fancybox-width`将默认为`400px`。

### 3.3 嵌套导入
跟原生的css不同，sass允许@import命令写在css规则内。这种导入方式下，生成对应的css文件时，局部文件会被直接插入到css规则内导入它的地方

~~~css
/*  blue-theme 文件 */
aside {
  background: blue;
  color: white;
}


/* 使用文件 */
.blue-theme {@import "blue-theme"}
.blue-theme {
  aside {
    background: blue;
    color: #fff;
  }
}
~~~

### 3.4 原生 css 导入
由于sass兼容原生的css，所以它也支持原生的CSS `@import`。
尽管通常在sass中使用`@import`时，sass会尝试找到对应的sass文件并导入进来，但在下列三种情况下会生成原生的CSS`@import`，这会造成浏览器解析css时的额外下载：
+ 被导入文件的名字以.css结尾；
+ 被导入文件的名字是一个URL地址（比如http://www.sass.hk/css/css.css），由此可用谷歌字体API提供的相应服务；
+ 被导入文件的名字是CSS的url()值。

so 不能用sass的@import直接导入一个原始的css文件，因为sass会认为你想用css原生的@import。

但是，因为sass的语法完全兼容css，所以你可以**把原始的css文件改名为.scss后缀，即可直接导入了**


## 4. 静默注释

sass 提供了一种不同于 css 标准的注释格式 `//` , 在编译成 css 后，这些用 `//` 的注释语句都会被抹去

## 5. 混合器 `@mixin` `@include`
需要大段大段的重用样式的代码，可以通过sass的混合器实现大段样式的重用。

混合器使用`@mixin`标识符定义。这个标识符给一大段样式赋予一个名字，可以通过引用这个名字重用这段样式

~~~css
@mixin rounded-corners {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}

notice {
  background-color: green;
  border: 2px solid #00aa00;
  @include rounded-corners;
}

/* less */
.post a {
  color: red;
  rounded-corners();
}
~~~

### 5.1 使用混合器的时间
混合器是在样式表中应用的。混合器是展示性的描述，用来描述一条css规则应用之后会产生怎样的效果。

混合器和类配合使用写出整洁的html和css，因为使用语义化的类名亦可以帮你避免重复使用混合器。

### 5.2 混合器中css规则
不仅可以包含属性，也可以包含css规则，包含选择器和选择器中的属性
~~~css
@mixin no-bullets {
  list-style: none;
  li {
    list-style-image: none;
    list-style-type: none;
    margin-left: 0px;
  }
}
~~~

### 5.3 给混合器传参
可以通过在@include混合器时给混合器传参，来定制混合器生成的精确样式

~~~css
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}

a {
  @include link-colors(blue, red, green);
}

/* 通过$name: value的形式指定每个参数的值 */
a {
    @include link-colors(
      $normal: blue,
      $visited: green,
      $hover: red
  );
}
~~~

### 5.4 默认参数值
 参数默认值使用$name: default-value的声明形式，默认值可以是任何有效的css属性值，甚至是其他参数的引用
~~~css
@mixin link-colors(
    $normal,
    $hover: $normal,
    $visited: $normal
  ){
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}

~~~

## 6. 使用选择器继承 `@extend`
> if 想重用语义化

~~~css
.error {
  border: 1px solid red;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
~~~
> `.seriousError`将会继承样式表中任何位置处为`.error`定义的所有样式
== 
以`class="seriousError"` 修饰的html元素最终的展示效果就好像是`class="seriousError error"`。

### 6.1 什么时候使用继承
**混合器**主要用于展示性样式的重用，而类名用于语义化样式的重用。因为**继承是基于类**的（有时是基于其他类型的选择器），所以继承应该是建立在语义化的关系上。

### 6.2 继承的高级用法
最常用的一种高级用法是继承一个html元素的样式。尽管默认的浏览器样式不会被继承，因为它们不属于样式表中的样式，但是你对html元素添加的所有样式都会被继承

~~~css
.disabled {
  color: gray;
  @extend a;
}
.disabled {
  color: gray;
  @extend a;
}
~~~
> if 一条样式规则继承了一个复杂的选择器，那么它只会继承这个复杂选择器命中的元素所应用的样式


### 6.3 继承的工作细节
`@extend`背后最基本的想法是: 
如果`.seriousError @extend .error`， 那么样式表中的任何一处`.error`都用`.error.seriousError`这一选择器组进行替换。这就意味着相关样式会如预期那样应用到`.error`和`.seriousError`

> + 跟混合器相比，**继承生成的css代码相对更少**。
因为**继承仅仅是重复选择器，而不会重复属性**，所以使用继承往往比混合器生成的css体积更小。如果你非常关心你站点的速度，请牢记这一点。
+ **继承遵从css层叠**的规则。当两个不同的css规则应用到同一个html元素上时，并且这两个不同的css规则对同一属性的修饰存在不同的值，根据权重选择。
+ **混合器本身不会引起css层叠的问题**，因为混合器把样式直接放到了css规则中，而继承存在样式层叠的问题。被继承的样式会保持原有定义位置和选择器权重不变。

