---
title: webpack 学习笔记
date: 2021/9/13
updated: 2021/9/15
comments:
tags:
 - webpack
categories:
 - learningNotes
layout:
permalink:
meta: true
---


## 0. atguigu 视频 的第三方包的版本

#### 0.1 创建项目 初始化项目

npm init

#### 0.2 包版本

##### 全局安装 -g 和 本地安装 -D : 

+ webpack@4.41.6           我下的—5.52.1

+ webpack-cli@3.3.11                 —4.8.0

##### loader 和一些 pulgin 插件：

###### 1. 处理css：

css-loader@3.4.2   —6.2.0

style-loader@1.1.3  —3.2.1

###### 2. 处理less

less-loader@5.0.0   —10.0.1

less@3.11.1  —4.1.1

###### 3. 处理html

html-webpack-plugin@3.2.0   —5.3.2

###### 4. 处理img

url-loader@3.0.0   —4.1.1

file-loader@5.0.2   —6.2.0

###### 5. 处理html img

html-loader@0.5.5   —2.1.2

###### 6. devServer

webpack-dev-server@3.10.3   —4.2.0

###### 7. 分离css

mini-css-extract-plugin@0.9.0

###### 8. css兼容

postcss-roader@3.0.0

postcss-preset-env@6.7.0

###### 9. css压缩

optimize-css-assets-webpack-plugin@5.0.3

###### 10. js兼容

babel-roader@8.0.6

@babel-preset-env@7.4.8

@babel/core@7.8.4

@babel-polyfill@7.8.3

core-js@3.6.4

###### 11. pwa

workbox-webpack-plugin@5.0.0

serve@11.3.0

###### 12. 多进程

thread-roader@2.1.3

###### 13. dll

add-asset-html-webpack-plugin@3.1.3



terser-webpack-plugin@2.3.5



## 1.webpack 开发环境配置

### 1.1 基本配置

~~~js
const { resolve } = require('path');

module.exports = {
  // 入口起点
  entry: './src/js/index.js',
  // 输出
  output: {
    filename: 'bulit.js',
    path: resolve(__dirname, 'build')
  },
  // loader配置
  module: {
    rules: [
      // 详细loader配置
    ]
  },
  // plugins插件的配置
  plugins: [

  ],
  mode: 'development',
  // mode: 'production',
}
~~~

### 1.2 loader的配置

#### 1.2.1 css

~~~js
 // 配置css
      {
        // 匹配了哪些文件
        test: /\.css$/,
        // 使用了哪些loader进行处理
        use: [
          // use数组中 loader的执行顺序:从右到左  从下到上
          // 创建 style 标签，将js中的样式资源插入到head中
          'style-loader',
          // 将css文件变化 commonjs 模块加载到js中 里面的内容样式是字符串
          'css-loader'
        ]
      },
~~~



#### 1.2.2 less

~~~js
 //配置less 要加上上面配置的css
      {
        test: /.\less$/,
        // 最后一个 将less 编译成css 文件 
        // 需要下载 less-loader less
        use: ['style-loader', 'css-loader', 'less-loader']
      },
~~~



#### 1.2.3 图片

~~~js
//配置图片资源  默认处理不了 html 中的img
        {
          test: /\.(jpg|png|gif|jpeg)$/,
          // 使用一个loader时 可以直接用下面这种方法
          // 下载 url-loader file-loader-> 不用下载
          loader: 'url-loader',
          options: {
            publicPath: './img',
            outputPath: 'img',
            // 图片大小小于 8kb 就会被base64 处理
            // 优点：减少请求数量  减轻服务器压力
            // 缺点：图片体积会更大 文件请求速度更慢
            limit: 8 * 1024,
            // fallback: resolve('file-roader'),
            // 对图片进行重命名
            name: 'img/[name][hash:10].[ext]',
            // 因为url-loader 默认 es6 模块化解析 而html-loader引入图片是commonjs
            // 解析时 会出现 [object Module]
            // 关闭 url-loader 的es6 模块化 使用 commonjs
            esModule: false,
          },
        //或者
        // use: {
        //   loader: 'url-loader',
        //   options: {
        //     publicPath: './img',
        //     outputPath: 'img/',
        //     limit: 8192,
        //     esModule: false
        //   }
        // },
        type: 'javascript/auto',   //必要
~~~



#### 1.2.4 html中的图片

~~~js
//在上面图片配置的基础上
 //处理 html 的img
      {
        test: /\.html$/,
        // 引入 img 从而被 url-loader处理
        loader: 'html-loader',
        options: {
          esModule: false,
        }
~~~



#### 1.2.5 html

~~~js
//plugins : 下载 -> 导入 -> 使用
const HtmlWebpackPlugin = require('html-webpack-plugin');
// plugins插件的配置
  plugins: [
    // html-webpack-plugin
    // if 直接 new...() 会创建一个空的html 自动引入打包输出的所有资源
    // if new...({ template:'url'}) 会有路径中的文件的结果
    new HtmlWebpackPlugin({
      template: './src/index.html'
    })
  ],
~~~



#### 1.2.6 热部署

~~~js
  // 最新 webpack-cil 于这个不匹配
  // 开发服务器 devServer 用来自动化 自动编译 打开浏览器 刷新
  // 特点 只会在内存中编译打包 不会有任何输出
  // 启动 npx webpack-dev-server  
  devServer: {
    // 项目构建后目录
    contentBase: resolve(__dirname, 'build'),
    // 启动gzip压缩
    compress: true,
    // 端口号
    port: 3000,
    // 自动打开浏览器
    open: true
  }
~~~



## 2.webpack 开发环境依赖

### 2.1 提取css 成单独文件

~~~js
const { resolve } = require('path'); 
const HtmlWebpackPlugin = require('html-webpack-plugin'); 
const MiniCssExtractPlugin = require('mini-css-extract-plugin'); 

module.exports = { 
    entry: './src/js/index.js', 
    output: { 
        filename: 'js/built.js', 
        path: resolve(__dirname, 'build') 
    },
    module: { 
        rules: [ { 
            test: /\.css$/, 
            use: [ 
                // 创建 style 标签，将样式放入 
                // 'style-loader', 
                // 这个 loader 取代 style-loader。作用：提取js中的css成单独文件 MiniCssExtractPlugin.loader, 
                // 将 css 文件整合到 js 文件中 
                'css-loader' ] 
        } ] 
    },
    plugins: [ 
        new HtmlWebpackPlugin({ 
            template: './src/index.html' 
        }), 
        new MiniCssExtractPlugin({ 
            // 对输出的 css 文件进行重命名 
            filename: 'css/built.css' 
        }) 
    ],
    mode: 'development' 
};
~~~



### 2.2 css兼容

~~~js
//webpack.config.js
{ 
    test: /\.css$/, 
        use: [ 
            MiniCssExtractPlugin.loader, 
            'css-loader', 
            { 
                loader: 'postcss-loader', 
                options: { 
                    ident: 'postcss', 
                    plugins: () => [ 
                        // postcss 的插件 
                        require('postcss-preset-env')() 
                    ] 
                } 
            } 
        ]
}

plugins: [ 
    new HtmlWebpackPlugin({ template: './src/index.html' }), 
    new MiniCssExtractPlugin({ 
        filename: 'css/built.css' 
    }) 
]
~~~

~~~js
//package.js
"browserslist": { 
    "development": [ 
        "last 1 chrome version", 
        "last 1 firefox version", 
        "last 1 safari version" 
    ],
    "production": [ 
        ">0.2%", 
        "not dead", 
        "not op_mini all" 
    ] 
}
~~~



### 2.3 css压缩

~~~js
plugins: [
    ... //同上
    new OptimizeCssAssetsWebpackPlugin() 
],
~~~



### 2.4 js语法检查 eslint

~~~js
rules: [ 
    /*语法检查： eslint-loader eslint 
    注意：只检查自己写的源代码，第三方的库是不用检查的 
    设置检查规则： package.json 中 eslintConfig 中设置~ 
    "eslintConfig": { "extends": "airbnb-base" } 
    airbnb --> eslint-config-airbnb-base eslint-plugin-import eslint 
    */
    { 
        test: /\.js$/, 
        exclude: /node_modules/, 
        loader: 'eslint-loader', 
        options: { 
            // 自动修复 eslint 的错误 
            fix: true 
        } 
    } 
]
~~~

~~~js
//package.json
"eslintConfig": { 
    "extends": "airbnb-base",
     "env": { 
         "browser": true 
     } 
}
~~~



### 2.5 js兼容

~~~js
rules: [ 
    { 
        test: /\.js$/, 
        exclude: /node_modules/, 
        loader: 'babel-loader', 
        options: { 
            // 预设：指示 babel 做怎么样的兼容性处理 
            presets: [ 
                [ '@babel/preset-env', 
                 { 
                     // 按需加载 
                     useBuiltIns: 'usage', 
                     // 指定 core-js 版本 
                     corejs: { version: 3 },
                     // 指定兼容性做到哪个版本浏览器 
                     targets: { chrome: '60', firefox: '60', ie: '9', safari: '10', edge: '17' } 
                 } 
                ] 
            ] 
        } 
    } 
]
~~~



### 2.6 js压缩

~~~js
// 生产环境下会自动压缩 js 代码 
mode: 'production'
~~~



### 2.7 html压缩

webpack5也能压缩html

~~~js
plugins: [ 
    new HtmlWebpackPlugin({ 
        template: './src/index.html', 
        // 压缩 html 代码 
        minify: { 
            // 移除空格 
            collapseWhitespace: true, 
            // 移除注释 
            removeComments: true 
        } 
    }) 
],
mode: 'production'
~~~



## 3. webpack优化配置

### 3.1 HMR

~~~js
mode: 'development', 
devServer: { 
    contentBase: resolve(__dirname, 'build'), 
    compress: true, 
    port: 3000, 
    open: true, 
    // 开启 HMR 功能 
    // 当修改了 webpack 配置，新配置要想生效，必须重新 webpack 服务 
        hot: true 
}
~~~



### 3.2 **source-map**

~~~js
devtool: 'eval-source-map'
~~~



### 3.3 OneOf

~~~js
// 以下 loader 只会匹配一个 // 注意：不能有两个配置处理同一种类型文件 
oneOf: [
    ..处理css less img js...
]
~~~



### 3.4 缓存

~~~js
	/*
	正常来讲，一个文件只能被一个 loader 处理。 
	当一个文件要被多个 loader 处理，那么一定要指定 loader 执行的先后顺序： 
	先执行 eslint 在执行 babel 
	*/
	{ 
        test: /\.js$/, 
        exclude: /node_modules/, 
        loader: 'babel-loader', 
        options: { 
            // 预设：指示 babel 做怎么样的兼容性处理 
            presets: [ 
                [ '@babel/preset-env', 
                 { 
                     // 按需加载 
                     useBuiltIns: 'usage', 
                     // 指定 core-js 版本 
                     corejs: { version: 3 },
                     // 指定兼容性做到哪个版本浏览器 
                     targets: { chrome: '60', firefox: '60' } 
                 } 
                ] 
            ],
            // 开启 babel 缓存 
            // 第二次构建时，会读取之前的缓存 
            cacheDirectory: true
        } 
    } 
~~~



### 3.5 tree shaking 树摇

前提：必须使用 es6 模块化   开启 production 环境

作用：减少代码体积

~~~json
//package.json
//所有代码都没有副作用 都可以进行树摇
//问题：可能会把 css/ @babel/polyfill 文件干掉
"sideEffects":false,
//对数组里的文件会忽略该操作
"sideEffects":["*.css","*.less"]
~~~



### 3.6 code spilt 代码分割

1. 利用多入口  打包成多个js文件

   ~~~js
   entry:{
      index: ’./src/js/index.js’,
      test: ’./src/js/test.js’
   }
   ~~~

2. 单/多入口 与 optimization 配合

   ~~~js
   entry: ‘./src/index.js’,
   // 或者 多入口
   
   //1. 可以将node_modules中代码单独打包一个chunk最终输出
   //2. 自动分析多入口chunk中，有无公共文件 if有 就会打包成单独一个chunk
   optimization:{
   	splitChunks:{
   		chunk: ’all’
   	}
   }
   ~~~

3. 在单入口 与 optimization 配合 页面的 js 文件里面使用 import

   通过js代码，让某某个文件被单独打包成一个chunk

   import 动态导入语法：能将某个文件单独打包

   ~~~js
   //  /* webpackChunkName: 'test' */ webpack.config.js 中 的[name] == test
   import(/* webpackChunkName: 'test' */'./test').then(({mul,count}) => {
       //文件加载成功
   }).catch(() => {
       //文件加载失败
   })
   ~~~

   

### 3.7 **lazy loading**

懒加载：文件需要使用是才加载

预加载 prefetch：使用之前 提前记载js文件  等其他资源加载完毕 浏览器空闲了 再偷偷加载

eg

~~~js
btn.onclick = function(){
    //懒加载
    import(/* webpackChunkName: 'test' */'./test').then(({mul,count}) => {
    	//文件加载成功
	})
    //预加载
    import(/* webpackChunkName: 'test' , webpackPrefetch: true */'./test').then(({mul,count}) => {
    	//文件加载成功
	})
}
~~~



### 3.8 pwa

现在好像已经被淘汰了

pwa 渐进式网络开发应用程序  离线可访问

~~~js
// webpack.config.js
new WorkboxWebpackPlugin.GenerateSW{
    //1.帮助serviceworker快速启动
    //2.删除旧的 serviceworker
    
    //生成一个serviceworker 配置文件
    clicentsClaim: true,
    skipWaiting: true
}
~~~

~~~js
// index.js
//注册 serviceWorker
//处理兼容性问题
if('serviceWorke' in navigator){
    window.onload(() => {
        navigator.serviceWorker
        .register('/service-worker.js')
        .then(() => {
            log('成功')
        })
        .catch(() => {
            log('失败')
        })
    })
}
~~~

~~~js
//因为eslint不认识 window navigator全局变量
//解决 需要修改 package.json eslintConfig 配置
"env":{
    "browser":true //支持浏览器端全局变量
}
~~~

sw 必须运行在服务器上 

+ nodejs

+ npm i serve -g

  serve -s build 启动服务器 将build目录的所有资源 作为静态资源暴露出去



### 3.9 多进程打包

~~~js
{ 
    test: /\.js$/, 
    exclude: /node_modules/, 
    use: [{
        /*开启多进程打包。 
        进程启动大概为 600ms，进程通信也有开销。 
        只有工作消耗时间比较长，才需要多进程打包 */ 
            loader: 'thread-loader', 
            options: { 
                workers: 2 // 进程 2 个 
             }
        }]
}
~~~



### 3.10 **externals**

不打包 jquery 然后 在html 手动引入cdn

~~~js
mode: 'production', 
externals: { 
    // 拒绝 jQuery 被打包进来 
    jquery: 'jQuery' 
}
~~~



### 3.11 dll

使用dll 对某些 第三方库 进行单独打包

+ 运行webpack 时 默认查找webpack.config.js 
+ 所以运行 改文件应 `webpack --config webpack.dll.js`

~~~js
// webpack.dll.js
...
const webpack = require("webpack");

entry:{
    //最终打包生成的[name] = jquery
    //['jquery'] = 要打包的库
    jquery:['jquery']
}，
output:{
    filename:'[name].js',
    path:resolve(__dirname,'dll'),  //打包后的位置
    library:'[name]_[hash]'  //打包的库里面向外暴露出去的内容的名字
},
plugins:[
    // 告诉webpack 不用再打包了
    //打包生成一个manifest.js  提供和jq 的映射
    new webpack.DllPlugin({
        name:'[name]_[hash]',  //映射库的暴露内容名称
        path:resolve(__dirname,'dll/nanifest.json')  //输出文件路径
    })
]

~~~

之后再运行webpack 就不会再重复打包jq了

~~~js
//webpack.config.js
plugins: [ 
    new HtmlWebpackPlugin({ template: './src/index.html' }), 
    // 告诉 webpack 哪些库不参与打包，同时使用时的名称也得变~ 
    new webpack.DllReferencePlugin({ 
        manifest: resolve(__dirname, 'dll/manifest.json') 
    }), 
    // 将某个文件打包输出去，并在 html 中自动引入该资源 
    new AddAssetHtmlWebpackPlugin({ 
        filepath: resolve(__dirname, 'dll/jquery.js') 
    })
]
~~~



### 3.12 优化总结

1. 开发环境性能优化

   + 优化打包构建速度

     HMR

   + 优化代码调试

     source-map

2. 生成环境性能优化

   + 优化打包构建速度

     OneOf

     babel缓存

     多进程打包

     externals

     dll

   + 优化代码运行的性能

     缓存(hash–chunkhash–contenthash)

     tree shaking

     code split

     懒加载/预加载

     pwa



## 4. webpack 配置详情

### 4.1 entry 

入口起点

1. String  -> ‘./src/index.js’

   单入口  打包形成一个chunk 输出一个 bundle 文件

   chunk 名称默认是 main

2. array  –> [‘./src/index.js’, ‘./src/add.js’]

   多入口  所有入口文件形成一个chunk 输出一个 bundle 文件

3. object  -> { index : ‘./src/index.js’, add:  ‘./src/add.js’ }

   多入口 几个入口就几个chunk 输出几个bundle

   chunk 名称为 key值

4. 特殊用法

   {

   ​	 index :[‘./src/index.js’, ‘./src/add.js’],    //一个chunk 一个bundle

   ​	 add:  ‘./src/add.js’    //一个chunk 一个bundle

   }



### 4.2 output

~~~js
output: { 
    // 文件名称（指定名称+目录） 
    filename: 'js/[name].js', 
    // 输出文件目录（将来所有资源输出的公共目录） 
    path: resolve(__dirname, 'build'), 
    // 所有资源引入公共路径前缀 --> 'imgs/a.jpg' --> '/imgs/a.jpg' 
    publicPath: '/', 
    chunkFilename: 'js/[name]_chunk.js', // 非入口 chunk 的名称 
    // library: '[name]', // 整个库向外暴露的变量名 
    // libraryTarget: 'window' // 变量名添加到哪个上 browser 
    // libraryTarget: 'global' // 变量名添加到哪个上 node 
    // libraryTarget: 'commonjs' 
},
~~~



### 4.3 module

~~~js
module: { 
    rules: [ 
    // loader 的配置 
        { 
            test: /\.css$/, 
            // 多个 loader 用 use
            use: ['style-loader', 'css-loader'] 
        },
        { 
            test: /\.js$/, 
            // 排除 node_modules 下的 js 文件 
            exclude: /node_modules/, 
            // 只检查 src 下的 js 文件 
            include: resolve(__dirname, 'src'), 
            // 优先执行 enforce: 'pre', 
            // 延后执行 // enforce: 'post', 
            // 单个 loader 用 loader 
            loader: 'eslint-loader', 
            options: {} 
        },
        { 
            // 以下配置只会生效一个 
            oneOf: [] 
        } 
    ] 
},
~~~



### 4.4 resolve

~~~js
resolve: { 
    // 配置解析模块路径别名: 优点简写路径 缺点路径没有提示 
    alias: { 
        $css: resolve(__dirname, 'src/css') 
    },
     // 配置省略文件路径的后缀名 
     extensions: ['.js', '.json', '.jsx', '.css'], 
     // 告诉 webpack 解析模块是去找哪个目录 
      modules: [resolve(__dirname, '../../node_modules'), 'node_modules'] 
}
~~~



### 4.5 dev server

~~~js
devServer: { 
    // 运行代码的目录 
    contentBase: resolve(__dirname, 'build'), 
    // 监视 contentBase 目录下的所有文件，一旦文件变化就会 reload 
    watchContentBase: true, 
    watchOptions: { 
        // 忽略文件 
        ignored: /node_modules/ 
    },
    compress: true, // 启动 gzip 压缩  
    port: 5000,     // 端口号
    host: 'localhost',     // 域名 
    open: true,     // 自动打开浏览器 
    hot: true,     // 开启 HMR 功能  
    clientLogLevel: 'none',    // 不要显示启动服务器日志信息
    // 除了一些基本启动信息以外，其他内容都不要显示 
    quiet: true, 
    overlay: false,// 如果出错了，不要全屏提示~
    // 服务器代理 --> 解决开发环境跨域问题 
    proxy: { 
        // 一旦 devServer(5000)服务器接受到 /api/xxx 的请求，就会把请求转发到另外一个服务器 (3000)
        '/api': { 
            target: 'http://localhost:3000', 
            // 发送请求时，请求路径重写：将 /api/xxx --> /xxx （去掉/api） 
            pathRewrite: { '^/api': '' } 
        } 
    }
}
~~~



### 4.6 **optimization**

~~~js
optimization: { 
    splitChunks: { 
        chunks: 'all' 
        // 后面默认值，可以不写~ 
    },
    // 将当前模块的记录其他模块的 hash 单独打包为一个文件 runtime 
    // 解决：修改 a 文件导致 b 文件的 contenthash 变化 
    runtimeChunk: { 
        name: entrypoint => `runtime-${entrypoint.name}` 
    },
    minimizer: [ 
        // 配置生产环境的压缩方案：js 和 css 
        new TerserWebpackPlugin({ 
            // 开启缓存 
            cache: true, 
            // 开启多进程打包 
            parallel: true, 
            // 启动 source-map 
            sourceMap: true 
        }) 
    ] 
}
~~~

