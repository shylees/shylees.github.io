---
title: 包装对象与toString、valueOf隐式调用
date: 2021/12/17
updated: 2021/12/17
comments:
tags:
 - js
categories:
 - furtherNotes
layout:
permalink:
meta: true
---

>
> 问辉哥for in hasOwnPrototype 的时候，他问我String 有没有 toString 方法，
>
> 然后提到了包装对象 和 toString 的隐式调用
>
> 包装对象 https://www.jianshu.com/p/32465288e738
>
> 隐式转换 https://juejin.cn/post/6844903557968166926
>
> 隐式调用 https://juejin.cn/post/6844903749090017294


### 1. 包装对象

#### 1.1 定义

对象是 JavaScript 语言中最主要的数据类型，三种原始类型的值：Number、Boolean、String 的值，在一定的条件下也会**自动转为对象**，即**原始类型的 ” 包装对象 “** wrapper

所谓的包装对象，指的是与数值、字符串、布尔值分别相应的 Number、String、Boolean 三个原生对象。这三个原生对象可以把原始类型的值包装成对象。

```javascript
var v1 = new Number(123);
var v2 = new String('abc');
var v3 = new Boolean(true);

typeof v1 // "object"
typeof v2 // "object"
typeof v3 // "object"

v1 == 123 // true
v2 == 'abc' // true
v3 == true // true

v1 === 123 // false
v2 === 'abc' // false
v3 === true // false
```



#### 1.2 设计目的

包装对象的**设计目的**：

1. 使得”**对象**“这种类型**可以覆盖 JavaScript 所有的值**，整门语言有一个**通用的数据模型**
2. 使得**原始类型**的值有办法**调用自己的方法**



#### 1.3 普通函数 和 构造函数 使用

+ 作为普通函数调用（String(123) ）：将**任意类型**的值转为**原始类型**的值
+ 作为构造函数使用（ new String(123) ）：可以将**原始类型**的值转为 **对象**

~~~js
String(123)      // "123"
new String(123)  // String {"123"}
~~~



#### 1.4 方法

##### 1.4.1 实例方法

三种包装对象都具有的、从 object 对象继承的方法：`valueOf()` 和 `toString()`

###### 1.4.1.1 valueOf()

返回包装对象**实例对应的原始类型**的值

~~~js
new Number(123).valueOf()  // 123
new String(123).valueOf()  // "123"
new Boolean(123).valueOf() // true
~~~



###### 1.4.1.2 toString()

返回**对应的字符串形式**

~~~js
new Number(123).toString() // "123"
new String(123).toString() // "123"
new Boolean(123).toString() // "true"
~~~



##### 1.4.2 原始类型与实例对象的自动转换

某些场合，**原始类型的值会自动当作包装对象调用**，即调用包装对象的属性和方法，此时 JavaScript 引擎会**自动将原始类型的值转为包装对象的实例，并在使用后立即销毁实例**。

> eg：
>
> ~~~js
> let str = "abc";  // === new String(str)
> str.length // 3
> 
> str.x = 123
> str.x  // undefined
> ~~~
>
> 如上述的 str 是一个字符串，本身不是对象，不能调用 length 属性。JavaScript 引擎**自动将其转为包装对象**，在这个对象上调用 length 属性。调用结束后，这个**临时对象就会被销毁**。**这就是原始类型与实例对象的自动转换**。
>
> 上述第二个例子，返回 undefined 的原因有：
>
> 1. 自动转换生成的包装对象是**只读的**
> 2. 调用结束后，包装对象的实例会销毁，意味着下次调用字符串属性时，调用的是一个**新的对象**
>
> 如果要为字符串添加属性，只有在其原型对象 String.prototype 上定义



##### 1.4.3 自定义方法

除了原生的实例方法，包装对象还可以自定义方法和属性，供原始类型的值直接调用

自定义的方法要加载 包装对象的 prototype 上

> eg：
>
> ~~~js
> Number.prototype.double = function(){
>     return this.valueOf() + this.valueOf();
> }
> (123).double()  // 246  //要加上圆括号，否则后面的点运算符会被解释成小数
> 123.0.double()
> 123..double()
> 123 .double()
> ~~~



#### 1.5 Boolean 对象

##### 1.5.1 概述

作为构造函数，其主要用于生成布尔值的包装对象实例

~~~js
let b = new Boolean(false);

typeof b // "object"
b.valueOf() // false

if(new Boolean(false)){  // 因为 得到的是一个对象 所有是 true
    console.log(true);
}

~~~

##### 1.5.2 类型转换作用

作为普通函数使用时，boolean 就单纯是一个工具方法**Boolean() 的 语法糖为 !!**

~~~js
Boolean(undefined) // false
Boolean(null) // false
Boolean(0) // false
Boolean('') // false
Boolean(NaN) // false

Boolean(1) // true
Boolean('false') // true
Boolean([]) // true
Boolean({}) // true
Boolean(function () {}) // true
Boolean(/foo/) // true
~~~



#### 1.6 Number 对象

##### 1.6.1 概述

作为构造函数时，用于生成值为数值的对象

作为普通/工具 函数时，可以将任何类型的值转为数值

~~~js
let n = new Number(1);
Number(true);  // 1
~~~

##### 1.6.2 静态属性

直接定义在 Number 对象的属性，而不是定义在实例上的，即要用 Number.xxx 访问

~~~js
Number.POSITIVE_INFINITY    // Infinity  无穷大
Number.NEGATIVE_INFINITY    // -Infinity 无穷小
Number.NaN                  // NaN 非数值

Number.MAX_VALUE            // 1.7976931348623157e+308 
Number.MAX_VALUE < Infinity // true

Number.MIN_VALUE            // 5e-324   最小正整数
Number.MIN_VALUE > 0        // true

Number.MAX_SAFE_INTEGER     // 9007199254740991   能精确表示的最大整数
Number.MIN_SAFE_INTEGER     // -9007199254740991  能精确表示的最小整数
~~~

##### 1.6.3 实例方法

有四个实例方法，都与数值转换指定格式有关

###### 1.6.3.1 Number.prototype.toString()

部署了自己的 toString 方法，用来将一个数值转换为字符串形式；

toString() 括号里可以接收一个参数，表示输出的进制。默认是十进制。

> 调用的时候也要注意 1.4.3 中的点运算符
>
> ~~~js
> (10).toString( /2/8/16);  "10/1010/12/a"
> ~~~

###### 1.6.3.2 Number.prototype.toFixed()

将一个数转为指定位数的小数（ 四舍五入 / 补零），然后返回这个小数对应的字符串



###### 1.6.3.3 Number.prototype.toExponential()

将一个数转为科学计数法形式，然后返回对应的字符串



###### 1.6.3.4 Number.prototype.roPrecision()

将一个数转为指定位数的有效数字（ 四舍五入 / 补零），然后返回对应的字符串

##### 1.6.4 自定义方法

在 Number.protorype 对象上自定义方法，会被 Number 的实例继承



#### 1.7 String 对象

##### 1.7.1 概述

作为构造函数：生成字符串对象，其为一个类似数组的对象（很像数组，但不是数组）

> 字符串`abc`对应的字符串对象，有数值键（`0`、`1`、`2`）和`length`属性，所以可以像数组那样取值

```javascript
new String('abc')
// String {0: "a", 1: "b", 2: "c", length: 3}

(new String('abc'))[1] // "b"
```

作为普通函数：将任意类型的值转为字符串



##### 1.7.2 静态方法

###### String.fromCharCode()

静态方法（即定义在对象本身，而不是定义在对象实例的方法），参数是一个或多个数值，代表 Unicode 码，返回值为这些码组成的字符串。

~~~js
String.fromCharCode() // ""
String.fromCharCode(97) // "a"
String.fromCharCode(104, 101, 108, 108, 111) // "hello"
~~~

> 该方法不支持 Unicode 码点大于`0xFFFF`的字符

##### 1.7.3 实例属性

###### String.prototype.length

##### 1.7.4 实例方法

###### String.prototype.charAt()

返回指定位置的字符：`‘abc’.charAt(1) == 'abc'[1] = 'b'`

###### String.prototype.charCodeAt()

返回字符串指定位置的 Unicode 码：`‘abc’.charCodeAt(1) == 98`

###### String.prototype.concat()

用于连接两个字符串，返回一个新字符串，不改变原字符串，可以有多个参数：`s1.concat(s2),'a'.concat('b', 'c')`

###### String.prototype.slice()

从原字符串取出子字符串并返回，不改变原字符串。它的第一个参数是子字符串的开始位置，第二个参数是子字符串的结束位置(不含)

###### String.prototype.substring()

同上

###### String.prototype.substr()

同上

###### String.prototype.indexOf()

用于确定一个字符串在另一个字符串中第一次出现的位置，返回结果是匹配开始的位置；

还可以接受第二个参数，表示从该位置开始向后匹配。

###### String.prototype.lastIndexOf()

从尾部开始匹配，第二个参数表示从该位置起向前匹配

###### String.prototype.trim()

去除字符串两端的空格，返回一个新字符串，不改变原字符串；

去除的不仅是空格，还包括制表符（`\t`、`\v`）、换行符（`\n`）和回车符（`\r`）

###### String.prototype.toLowerCase()，String.prototype.toUpperCase()

都返回一个新字符串，不改变原字符串

###### String.prototype.match()

确定原字符串是否匹配某个子字符串，返回一个数组，成员为匹配的第一个字符串。如果没有找到匹配，则返回`null`

返回的数组还有`index`属性和`input`属性，分别表示匹配字符串开始的位置和原始字符串

###### String.prototype.search()

基本等同于`match`，但是返回值为匹配的第一个位置。如果没有找到匹配，则返回`-1`

还可以使用正则表达式作为参数。

###### String.prototype.replace()

用于替换匹配的子字符串，一般情况下只替换第一个匹配（除非使用带有`g`修饰符的正则表达式）

###### String.prototype.split()

按照给定规则分割字符串，返回一个由分割出来的子字符串组成的数组

> ~~~js
> 'a|b|c'.split('|') // ["a", "b", "c"]
> 'a|b|c'.split('') // ["a", "|", "b", "|", "c"]
> 'a|b|c'.split() // ["a|b|c"]
> ~~~

###### String.prototype.localeCompare()

用于比较两个字符串。

返回一个整数，如果小于0，表示第一个字符串小于第二个字符串；如果等于0，表示两者相等；如果大于0，表示第一个字符串大于第二个字符串

还可以有第二个参数，指定所使用的语言（默认是英语

> ~~~js
> 'apple'.localeCompare('banana') // -1
> 'apple'.localeCompare('apple') // 0
> ~~~
>
> 

### 2. 隐式调用

简单说就是自动调用一些方法，而这些方法像钩子一样可以在外部修改，从而改变既定行为。

#### 2.1 数据类型转换 toString 和 valueOf

~~~js
let a = {
    i: 1,
    valueOf: function () {      // 改成 => 就不行 记得看 this 指向
        console.log("valueof")
        return this.i++;
    },
    toString: function () {
        console.log("tostring");
        return this.i++;
    }
}

a == 1  // "valueof"  true
Number(a)  // "valueof" 2
String(a)  // "tostring" "3"
Boolean(a) // true  因为是对象
a + 1     // "valueof" 5
String(a) + 1 // "toString" "61"

let b = {
    i: 1,
    valueOf: function () {
        console.log("valueof")
        return {};
    },
    toString: function () {
        console.log("tostring");
        return {};
    }
}

Number(b) // "valueof" "tostring" Uncaught TypeError
b == 1    // "valueof" "tostring" Uncaught TypeError
String(b) // "tostring" "valueof" Uncaught TypeError
b == "1"  // "valueof" "tostring" Uncaught TypeError
~~~

在相等 **==** 运算符 / 加号 **+** 操作中，（null 除外）对象会**先调用 valueOf** ，如果**返回值是对象，就会调用 toString**， 然后用返回的值进行比较 / 加号操作。

Number 和 String 方法中 ，**Number 会先调用 valueOf 后调用 toString**，**String 则相反**。

#### 2.2 DOM2 事件中的 handleEvent

~~~js
let eventObj = {
    a: 1,
    handleEvent: function(e){
        console.log(this, e);  // eventObj ， 事件对象
    }
}

document.addEventListener('click',eventObj);
~~~

addEventListener 第二个参数除了函数外还可以是一个对象，事件触发后会执行对象的 handleEvent 方法，方法执行时的 this 指向 eventObj，你可以把想传入的数据绑定在 eventObj 对象上

#### 2.3 JSON 对象 toJSON

#### 2.4 promise 对象的 then

#### 2.5 对象属性存取器 get 和 set

#### 2.6 遍历器接口 Symbol.iterator
