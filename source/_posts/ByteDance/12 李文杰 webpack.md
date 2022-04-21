---
title: 构建 webpack 知识体系 - 字节青训营
date: 2022/1/25
updated: 2022/1/25
comments:
tags:
 - webpack
 - 青训营
categories:
 - learningNotes
layout:
permalink:
meta: true
---

# 构建 webpack 知识体系

知识体系：https://gitmind.cn/app/doc/fac1c196e29b8f9052239f16cff7d4c7

## 1. 什么是webpack

本质上是一种前端资源编译、打包工具

+ 多份资源文件打包成一个Bundle
+ 支持 Babel、Eslint、TS、CoffeScript、Less、Sass
+ 支持模块化处理 css、图片等资源文件
+ 支持 HMR + 开发服务器
+ 支持持续监听、持续构建
+ 支持代码分离
+ 支持 Tree-shaking
+ 支持 Sourcemap
+ …

## 2. 使用 webpack

### 1. 示例

1. 安装

   `npm i -D webpack webpack-cli`

2. 编辑配置文件

   ~~~js
   // webpack.config.js
   module.exports = {
       entry:"main.js",
       output:{
           filename:"[name].js",
           path:path.join(__dirname,"./dist"),
       },
       module:{
           rules:[{
               test:/\.less$/i,
               use:['style-loader','css-loader','less-loader']
           }]
       }
   }
   ~~~

3. 执行编译命令

   `npx webpack`

### 2. 核心流程 — 极简化版

1. 入口处理 ： 从entry文件开始启动编译流程
2. 依赖解析 ：根据require、import等找到依赖资源
3. 资源解析 ：根据module配置，调用资源转移器，将css等非标准js资源转译为 js 内容
4. 资源合并打包 ：将转译后的资源内容合并打包为可直接在浏览器运行的js



### 3. 配置项概览

 使用 webpack 的方法：**基本围绕 “配置”展开，可分为**

+ 流程类：作用于流程中某个 or 若干个环节，直接影响打包效果的配置项
  + 输入：entry、context
  + 模块解析：resolve、externals
  + 模块转译：module
  + 后处理：optimization、mode、target
+ 工具类：主流程之外，提供更多工程化能力的配置项
  + 开发效率类：watch、devtool、devServer
  + 性能优化类：cache、performance
  + 日志类：stats、infrastructureLogging
  + 其他：amd、bail



配置项的使用频率：

+ entry/output
+ module/plugins
+ mode
+ watch/devServer/devtools

#### 3.1 处理 css – style-loader,css-loader

1. 安装loader
2. 编辑配置项

> loader 有什么作用？为什么要用到 css-loader、style-loader？
>
> A: 因为 webpack 只认识 js，为了处理非标准js资源，设计出的资源翻译模块 — loader，将资源翻译为标准 js
>
> 与在 html 文件中维护 css 相比，这种方式会有什么优劣？
>
> 如何在webpack接入less、sass、stylus？

#### 3.2 处理 js – 接入 Babel

1. 安装依赖 `npm i -D @babel/core @babel/preset-env babel-loader`
2. 声明 output
3. 执行 npx webpack

> Babel 有什么功能
>
> Babel 与 webpack 分别解决了什么问题

#### 3.3 生成 HTML

1. 安装依赖 `npm i -D html-webpack-plugin`
2. 声明产物出口 output
3. 使用插件 `plugins:[new HtmlWebpackPlugin]`
4. 执行 `npx webpack`

> 相比于手工维护的 HTML 内容，这种自动生成的方式有什么优缺点？

### 4. 工具线

#### 4.1 HMR 浏览器热替换

1. 更改配置项 devServer: { hot: true, open: true } ,watch: true // 持续监听，生成新的
2. 执行 npx webpack server

#### 4.2 Tree-Shaking 树摇

开启树摇

更改配置项：

+ mode: ”production“,

+ optimization: { usedExports: true }

> 对工具库如Lodash有奇效
>
> require 不能进行 tree-shaking
>
> 

#### 4.3 其他工具

+ 缓存 webpack5后的缓存效果才比较好
+ Sourcemap
+ 性能监控
+ 日志
+ 代码压缩
+ 分包
+ …

> 还有哪些可被划分为”流程类“？
>
> 工具类配置具体有什么作用，包括 devtool/cache/stat



## 3. 理解 Loader

**为了处理非标准 js 资源，设计出资源翻译模块 — Loader，用于将资源翻译为标准  JS**



### 3.1 使用：

1. 安装 Loader
2. 添加 module 处理 需要翻译的文件



### 3.2 认识Loader

#### 链式调用

前面的输出 == 后面的输入，每个 loader 比较内聚，

以 处理 less 文件为例：

+ less-loader：实现 less 到 css 的转换
+ css-loader：将 css 包装成类似于 module.exports = “${css}” 的内容，包装后的内容符合js 语法
+ style-loader：将 css 模块包进 require 语句，并在运行时调用 injectStyle 等函数将内容注入到页面的style标签



debug工具： ndb `ndb npx webpack`

特点：

+ 链式调用
+ 支持异步执行
+ 分 normal、pitch 两种模式



#### 如何编写

~~~js
// loader.js
module.exports = function(source,sourceMap,data?){
    // source 为 文件输入
    // 可能是文件内容，也可以是上一个 loader 处理结果
    return source;
}
~~~

~~~js
// eslint-loader/index.js
import getOptions from './getOptions';
import Linter from './Linter'
import cacheLoader from './cacheLoader';

export defalut function loader(content, map){
    const options = getOptions(this);
    const linter = new Linter(this, options);
    
    this.cacheable();
    
    if(options.cache){
        cacheLoader(linter, content, map);
        return;
    }
    
    linter.printOutput(linter.lint(content));
    this.callback(null, content, map);
}
~~~



参考：Webpack 原理系列七：如何编写loader

#### 常见loader

理解

+ js：babel、eslint、ts、buble、vue、angular2-template   - loader
+ css：css、style、less、sass、stylus、postcss - loader
+ html：html、pug、pisthtml - loader
+ assets：file、val、url、json5 - loader



> loader 输入是什么？要求输出的是什么？
>
> loader 的链式调用是什么意思？如何串联多个 loader？
>
> loader 中如何处理异步场景？



## 4. 理解插件

### 4.1 插件是什么，为什么这么设计？

插件架构精髓：对扩展开放、对修改封闭



### 4.2 理解插件

插件围绕钩子展开

~~~js
class SomePlugin{
    apply(compiler){
        compiler.hooks.thisCompilation.tap('SomePlugin', compilation => {
            
        })
    }
}
~~~



钩子的核心信息：

1. 时机：编译过程的特定节点，Webpack 会以钩子的形式通知插件此刻正在发生什么事请
2. 上下文：通过 tapable 提供的回调机制，以参数方式传递上下文信息；
3. 交互：在上下文参数对象中附带了很多存在 side effect 的交互接口，插件可以通过这些接口改变



~~~js
class EntryPlugin{
    apply(compiler){
        compiler.hooks.compilation.tap( // 时机 compiler.hooks.compilation
            'EntryPlugin',
             (compilation,{ normalModuleFactory }) => {  // 参数 compilation
                 compilation.dependencyfactories.set(  // 交互 dependencyfactories.set
                 	EntryDenpendency,
                    noemalModuleFactory
                 );
             });
        
        compiler.hooks.make.tapAsync('EntryPlugin', (compilation, callback) => {
            const {entry,options, context } = this;
            const dep = EntryPlugin.createDependency(entry, options);
            compilation.addEntry(context, dep, options, err => {
                callback(err);
            })
            
        })
    }
}
~~~



> loader 与插件有什么区同点？
>
> ”钩子“有什么作用？如何监听钩子函数？



## 5. 学习方法

1. 入门应用
   + 理解打包流程
   + 熟练掌握常用配置项、loader、插件的使用方法、能够灵活搭建集成 Vue、React、Babel、Eslint、Less、Sass、图片处理等工具的webpack环境
   + 掌握常见脚手架工具，例如 Vue-cli、create-react-app、@angular/cli
2. 进阶
   + 理解Loader、Plugin机制，能够自行开发 Webpack 组件
   + 理解常见性能优化手段，并能用于解决实际问题
   + 理解前端工程化概念与生态现状
3. 大师级
   + 阅读源码，理解 编译、打包原理，能参与共建

## Q&A

##### 面试掌握程度？

+ loader 有什么作用，怎么写 loader、常见的 loader有什么
+ 怎么写一个 插件
+ bundle、chunk、module 是什么含义？

##### require 与 import 导入的区别？

+ 动态/静态代码

##### webpack 与 vite ？

+ vite：速度快 ，无 bundle，对开发性友好
+ webpack：生态成熟，

##### loader 与 plugin 区别？

+ loader：内容翻译为js
+ plugin：没有明确输入输出，作用于整个生命周期，在任意时间修改任意webpack组件

##### webpack 与 rollup

+ webpack 更重，应对浏览器
+ rollup 用来构建npm包，扩展性弱，应对库的场景

##### webpack 优化

##### npx 与 npm









































