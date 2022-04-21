---
title: 06 vue-router
date: 2021/3/23
updated: 2021/3/29
comments:
tags:
 - vue
categories:
 - learningNotes
layout:
permalink:
meta: true
---


[toc]

# (2021/03/23-29 p100-118)

## 1.认识路由

### 1.1 路由

路由：通过互联的网络把信息从源地址传输到目的地的活动

路由器提供了两种机制：路由和转送

​     路由是决定数据包从来源到目的地的路径

​     转送将输入端的数据转移到合适的输出端

路由表：本质上就是一个映射表，决定了数据包的指向

### 1.2 前端渲染 后端渲染(p101)

后端渲染：jsp：Java serve page

后端路由：后端处理url和页面之间的映射关系

前后端分离：ajax

SPA单页面富应用：在前后端分离的基础上加了一层前端路由

前端路由的核心：改变url，但页面不进行整体刷新

### 1.3 url的hash和HTML5的history

如何实现改变url，不刷新页面

#### 1.3.1 URL的hash

url的hash也就是锚点（#），本质上是改变window.location的herf属性

可以通过直接赋值location.hash来改变href，但是页面不发生刷新

<img src="https://s1.328888.xyz/2022/04/09/XapI2.png" alt="image-20210323202246058" style="zoom:67%;" />

#### 1.3.2 HTML5的history

+ `history.pushState({},'','home')`

  > `history.back()`可以回退  左箭头也能回退

+ `history.replaceState({},'','home')`

  > 不能回退

+ `history.go() `

  > `history.go(-1) == history.back()`
  >
  > 正数前进 负数后退

+ `history.forward()`

  > `history.forward() == history.go(1) `

## 2.vue-router基本使用

### 2.1 认识vue-router

目前前端流行的三大框架，都有自己的路由实现：

+ Angular的ngRouter
+ React的ReactRouter
+ Vue的vue-router

vue-router是Vue.js官方的路由插件，和vue.js是深度集成的，适合用于构建单页面应用

官网：https://router.vuejs.org/zh/

vue-router是基于路由和组件的

+ 路由用于设定访问路径，将路径和组件映射起来
+ 在vue-router的单页面应用中，页面的路径的改变就是组件的切换



### 2.2 安装和使用vue-router

#### 2.2.1安装

步骤一：安装

`npm install vue-router --save`

步骤二：在模块化工程中使用它（因为是一个插件，所以可以通过Vue.use()来安装路由功能）

1. **导入**路由对象，并且**调用Vue.use(VueRouter)**
2. 创建**路由实例**，并且传入路由**映射配置**
3. 在**Vue实例**中**挂载**创建的**路由实例**

~~~js
//router/index.js 搭建路由的框架

//配置路由相关的信息
import Vue from 'vue'
import Router from 'vue-router'

//1.通过Vue.use(插件),安装插件
Vue.use(Router)

//2.创建VueRouter对象

const routes = [{

}]

const router = new Router({
    //配置路由和组件之间的应用关系
    routes
})

//3.将其传入vue实例中
export default router

~~~

~~~js
//main.js

import Vue from 'vue'
import App from './App'
//router文件夹 会自动导入index
import router from './router'

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
    el: '#app',
    router,
    //将路由传入vue实例
    render: h => h(App)
})
~~~

#### 2.2.2使用(p104-110)

##### 1.使用步骤

使用vue-router的步骤：

1. 创建路由组件

2. 配置路由映射，组件和路径映射的关系

3. 使用路由：通过`<router-link>`和`<router-view>`

   > `<router-link>`该标签是一个vue-router中已经内置的组件，会被渲染成一个`<a>`标签

   > `<router-view>`	该标签会根据当前路径，动态渲染出不同的组件

   > 网页的其他内容，比如顶部的标题/导航，等会和`<router-view>`处于同一个等级

   > 在路由切换时，切换的是`<router-view>`挂载的组件，其他内容不会发生改变

~~~vue
//创建新的组件
//Home.vue

<template>
	<div>
		<h2>我是首页</h2>
	</div>
</template>

<script>
export default {
	name: "Home",
};
</script>

<style>
</style>

//About.vue
<template>
	<div>
		<h2>我是关于</h2>
	</div>
</template>

<script>
export default {
	name: "About",
};
</script>

<style>
</style>
~~~

~~~js
//配置映射  index.js

//配置路由相关的信息
import Vue from 'vue'
import Router from 'vue-router'

//导入组件
import Home from '../components/Home.vue'
import About from '../components/About.vue'

//1.通过Vue.use(插件),安装插件
Vue.use(Router)

//2.创建VueRouter对象
const routes = [{
    path: '/home',
    component: Home
}, {
    path: '/about',
    component: About
}]

const router = new Router({
    //配置路由和组件之间的应用关系
    routes
})


//3.将其传入vue实例中
export default router
~~~

~~~vue
//要把组件写到App.vue才能渲染出来

<template>
	<div id="app">
		<!-- router-link 与a标签类似  会将a标签渲染在页面上 -->
		<router-link to="/home">首页</router-link>
		<router-link to="/about">关于</router-link>
		<!-- 决定组件展示在页面的位置 -->
		<router-view></router-view>
	</div>
</template>

<script>
export default {
	name: "App",
};
</script>

<style>
</style>
~~~

<img src='https://s1.328888.xyz/2022/04/09/Xa0jM.png'>

##### 2.路由的默认路径：

~~~js
//index.js
const routes = [{
    path: '/',   //  ‘/’ 加不加都可以
    redirect: '/home'
}, {
    path: '/home',
    component: Home
}, {
    path: '/about',
    component: About
}]
~~~

> 在routes中又配置了一个映射，
>
> path配置的是根路径:/
>
> redirect是重定向，也就是将根路径重定向到/home的路径下，这样就可以打开就默认是home了

##### 3.修改url的展示模式

> 因为默认是hash展示 会有# 没那么好看
>
> 把mode改成history 就可以没有#

~~~js
//index.js

const router = new Router({
    //配置路由和组件之间的应用关系
    routes,
    mode: 'history'
})
~~~

##### 4.router-link补充

属性：

1. **to**：用于指定跳转的路径

2. **tag**：指定渲染成什么组件

   > 例`<router-link tag='li'></router-link>`渲染成`<li></li>`

3. **replace**：不会留下history记录

   >  例`<router-link replace></router-link>`

4. **active-class**：当`<router-link>`对应的路由匹配成功时，会自动给当前元素设置一个` router-link-active`的`class`，设置`active-class`可以修改默认的名称

   > 例`<router-link active-class='active'></router-link>`

   > 如果很多要改的话 可以在
   >
   > ~~~js
   > //index.js
   > const router = new Router({
   >     //配置路由和组件之间的应用关系
   >     routes,
   >     mode: 'history', //改变url的展示方式
   >     linkActiveClass: 'active'  //修改默认的点击赋给的类名
   > })
   > ~~~

##### 5.不使用router-link，使用代码修改路径

~~~vue
//App.vue
<template>
	<div id="app">
		<h2>我是app组件</h2>
		<!-- router-link 与a标签类似  相当于将a标签渲染在页面上 -->
		<!-- <router-link to="/home">首页</router-link>
		<router-link to="/about">关于</router-link> -->
        <!-- 1.用标签直接写  加上触发指定方法 -->
		<button @click="homeClick">首页</button>
		<button @click="aboutClick">关于</button>

		<!-- 决定组件展示在页面的位置 -->
		<router-view></router-view>
	</div>
</template>

<script>
export default {
	name: "App",
    //2.定义方法
	methods: {
		homeClick() {
			//3.通过代码的方式修改路由  vue-router
			//push == pushState  可以返回
			// this.$router.push("/home");
			this.$router.replace("/home");
		}, 
		aboutClick() {
			this.$router.push("/about");
		},
	},
};
</script>

<style>
</style>

~~~

##### 6.动态路由的使用(p108)

+ 某些情况下，一个页面的path路径可能是不确定的，比如进入用户页面的时候，希望是
  + /user/aaaa  或者 /user/bbbb
  + 除了有前面的/user 外 ，后面还跟上了用户ID
  + 这种path和Component的匹配关系，称之为动态路由==》也是路由传递数据的一种方式

+ 步骤：

  1. **添加组件User.vue 添加路由映射** 

     > ~~~js
     > //不使用到动态路由的话
     > {
     >     path:'/user',
     >     component:User
     > }
     > 
     > //使用动态路由
     > {
     >     path:'/user/:id',
     >     component:User
     > }
     > ~~~

  2. **在User.vue组件中使用动态路由传递的数据**

     > ~~~vue
     > <template>
     > <div>
     >     //id 与上面 :后面的名称一样
     >     <h2>{{ $router.params.id }}</h2>
     >     
     >     //或者使用组件下面获取的数据
     >     <h2>{{ userId }}</h2>
     > </div>
     > </template>
     > 
     > <script>
     > export default {
     > 	name: "User",
     > 	computed: {
     > 		userId() {
     > 			return this.$route.params.userId;
     > 		},
     > 	},
     > };
     > </script>
     > ~~~

  3. **在展示的组件 App.vue添加 此信息**

     > ~~~vue
     > <template>
     > 	<div id="app">
     >         //直接添加  没有动态获取的id
     >         <router-link to="/user/zhangsan">用户</router-link>
     >         
     >         //需要获取下面动态数据 v-bind
     > 		<router-link :to="'/user/' + userId">用户</router-link>
     > 	</div>
     > </template>
     > 
     > <script>
     > export default {
     > 	name: "App",
     > 	data() {
     > 		return {
     > 			userId: "lisi",
     > 		};
     > 	},
     > }
     > </script>
     > ~~~

**$router : new出来的路由对象**

**$route : 处于活跃的路由**

~~~vue
//User.vue
<template>
	<div>
		<h2>我是用户界面</h2>
		<p>我是用户相关信息</p>
	</div>
</template>

<script>
export default {
	name: "User",
};
</script>

<style>
</style>
~~~

~~~js
//index.js
//1.导入 +
import User from '../components/User.vue'

//2.创建VueRouter对象 +
const routes = [{
    //放在第一个 是好习惯 而非固定要求
    path: '/',
    redirect: '/home'
}, {
    path: '/home',
    component: Home
}, {
    path: '/about',
    component: About
}, {
    path: '/user',
    component: User
}]

~~~

> 但是不能起到拼接userid的作用

~~~js
//index.js
 {
    path: '/user/:userId',  //只改这里 不会渲染user
    component: User
}

//App.vue
//没有动态
<router-link to="/user/zhangsan">用户</router-link>  
//在跳转的时候 添加/zhangsan

//动态获取 属性动态添加 下面的data
<router-link :to="'/user/' + userId">用户</router-link>

export default {
	name: "App",
	data() {
		return {
			userId: "lisi",
		};
	},
}
~~~

将用户id获取到 然后在User.vue使用

~~~vue
//User.vue
<template>
	<div>
		<h2>我是用户界面</h2>
		<p>我是用户相关信息</p>
		<h2>{{ userId }}</h2>   //!!!!
        <h2>{{ $route.params.userId }}</h2>
	</div>
</template>

<script>
export default {
	name: "User",
	computed: {
		userId() {
			//$route 拿到的是活跃的router
			//param：参数  userId : path: '/user/:userId' 的
			return this.$route.params.userId; //!!!
		},
	},
};
</script>

<style>
</style>
~~~

##### 7.路由懒加载(p109-110)

###### 7. 1vue-router打包文件的解析

> dist > static > index.html + css + js >app… + manifest… + vendor… 

在打包的时候 会自动把css文件 跟js文件分开放，不会像之前一样都放在一个js文件里

而js文件也有分不同的包

+ app : 当前应用程序开发的所有代码 （业务代码）
+ manifest：为打包的代码做底层（底层导入，导出 或者更复杂的操作）支撑
+ vendor：（提供商，第三方：vue/vue-router/axios/bs）第三方的东西

###### 7.2 认识路由懒加载

+ 官方解释：

当打包构建应用时，JavaScript包会很大，影响页面加载

如果能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应的组件，这样比较高效

+ 路由懒加载做的东西：
  + 主要作用：将路由对应的组件打包成一个个的js代码块
  + 只有在这个路由被访问到的时候，才加载对应的组件

###### 7.3 懒加载的方式

1. 结合Vue的异步组件和Webpack的代码分析（能认识就好）

   > ~~~js
   > const Home = resolve => { require.ensure(['../components/Home.vue'],() => { resolve(require('../components/Home.vue')) })}
   > ~~~

2. AMD写法：

   > ~~~js
   > const About = resolve => require(['../components/About.vue'],resolve);
   > ~~~

3. 在Es6中，我们可以有更加简单的写法来组织Vue异步组件和Webpack的代码分割(常用)

   > ~~~js
   > //代替导入操作  推荐使用
   > const Home = () => import('../components/Home.vue');
   > 
   > //也可以在路由那里直接使用
   > {
   >     path:'/home',
   >     component:() => import('../components/Home')
   > }
   > ~~~

这个时候在npm run build 打包的时候 就会发现 js文件夹下 多了3个js文件 

一个懒加载 一个js文件

## 3.vue-router嵌套路由

### 3.1 认识嵌套路由

+ 比如在home页面中，希望通过/home/news 和 /home/message访问一些内容

+ 一个路径映射一个组件，访问这两个路径也会分别渲染两个组件

+ 路径和组件的关系：

  ><img src='https://s1.328888.xyz/2022/04/09/Xa7J7.jpg' style="zoom:50%;" >

+ 实现嵌套路由的步骤：

  1. 创建对应的子组件，并且在路由映射中配置对应的子路由
  2. 在组件内部使用`<router-view>`标签

### 3.2 路由的嵌套使用

1. 新建两个HomeNews.vue  HomeMessage.vue 组件

2. 在index.js 配置上述两个vue的映射关系

   > ~~~js
   > //index.js
   > const routes = [
   >     {
   >     path: '/home',
   >     component: Home,
   >     children: [{
   >         path: 'news', //不用加 ‘/’  会自动给加上：'/home/news'
   >         component: HomeNews
   >     }, {
   >         path: 'message',
   >         component: HomeMessage
   >     }]
   > }
   > ]
   > ~~~

3. 子路由的显示

   > ~~~vue
   > //因为这些vue是显示在home页面的 所以在Home.vue页面中 添加`<router-view>`
   > <template>
   > 	<div>
   > 		<h2>我是首页</h2>
   > 		<!-- 要写完成路径 -->
   > 		<router-link to="/home/news">新闻</router-link>
   > 		<router-link to="/home/message">消息</router-link>
   > 		<router-view></router-view>
   > 	</div>
   > </template>  
   > ~~~

4. 默认显示新闻路径 配置映射

   > ~~~js
   > //index.js
   > children: [{
   >         path: '',
   >         redirect: 'news'
   >     }, {
   >         path: 'news', //不用加 ‘/’  会自动给加上：'/home/news'
   >         component: HomeNews
   >     }, {
   >         path: 'message',
   >         component: HomeMessage
   >     }]
   > ~~~

## 4.vue-router参数传递

### 4.1 传递参数的方式：params和query

+ params类型：只传一个简单的参数
  + 配置路由格式：`/router/:id`
  + 传递的方式：在path后面跟上对应的值`:to="'/user/' + userId"`
  + 传递后形成的路径：`/router/123`、`/router/abc`
+ query类型：
  + 配置路由格式：`/router` ，也就是普通配置
  + 传递的方式：对象中使用query的key作为传递方式
  + 传递后形成的路径：`/router?id=123`、`/router?id=abc`

### 4.2 query类型的使用

1. 创建新的组件 Profile.vue

2. 配置路由映射 index.js

   > 1. 导入：`const Profile = () => import ('../components/Profile.vue');`
   > 2. 配置映射：`{ path: '/profile', component: Profile }`

3. 添加跳转的`<router-link>` app.vue

   > `<router-link to="/profile">我的</router-link>`

4. 将参数传递到url

   > ~~~js
   > //index.js
   > //若想把{} 当成对象 就得加 v-bind 这样才能被当成语法去解析
   > <router-link :to="{path:'/profile',query:{name:'why',age:18,height1'1.88}}">我的</router-link>
   > 
   > //此时点击页面的'我的' 地址显示为  localhost:8080/profile?name=why&age=18&height=1.88
   > ~~~

5. 去参数渲染到页面上

   > ~~~vue
   > //Profile.vue
   > <template>
   > 	<div>
   > 		<h2>我是档案（我的）标题</h2>
   > 		<h2>{{ $route.query.name }}</h2>
   > 	</div>
   > </template>
   > ~~~

### 4.3 当不使用router-link而使用普通的标签 传递参数

~~~vue
//App.vue  单纯想要 跳转页面时
<template>
	<div id="app">
		<button @click="userClick">用户</button>
		<button @click="profileClick">我的</button>
		<router-view></router-view>
	</div>
</template>
<script>
export default {
	name: "App",
	data() {
		return {
			userId: "lisi",
		};
	},
	methods: {
		userClick() {
            //单纯想要 跳转页面时
			this.$router.push("/user/" + this.userId);
		},
		profileClick() {
            //想要传递数据时
			this.$router.push({
                path:'/profile',
                query:{
                    name:'kobe',
                    age:18,
                    height:1.88
                }
            });
		},
	},
};
</script>
~~~

### 4.4 vue-router ： router route 的由来(p111)

`$router`和`$route`是有区别的：

+ `$router`为VueRouter实例，想要导航到不同URL，则使用`$router.push`方法
+ `$route`为当前router跳转对象里面可以获取name、path、query、params等

所有的组件 继承 vue原型

## 5.vue-router导航守卫

实现点击 切换文档标题

### 5.1 生命周期函数

+ created：当组件被创建时调用
+ mounted：当模板渲染好后被调用
+ updated：页面更新时被调用

~~~vue
<script>
    //user组件为例
	export default{
        name:"User",
        computed:{
            userId(){
                return this.$route.params.id
            }
        },
        created(){
            console.log('created');
            //在这个时候可以做一些事情  比如将 title改为首页
            document.title='首页'
        }
    }
</script>
~~~

### 5.2 全局导航守卫

因为一个一个+需求代码 太麻烦了  

因为所有的跳转都是路由跳转，所以可以监听路由跳转的过程

监听：全局导航守卫

~~~js
//index.js
const routes =[
    {
        path:'/home',
        component:Home,
        //可以在每个路由加上这个属性
        meta:{
            title:'首页'
        }
    }
]
const router = new VueRouter({}); //后面

//前置守卫（guard） 跳转前调用
router.beforeEach((to, from, next) => {
    //从from 跳转到 to  to:route类型 活跃的路由
    //to.meta.title 第一个的时候是undefined
    document.title = to.matched[0].meta.title;
    next(); //必须调用next  不然不会进行下一步
})

//后置钩子hook 跳转之后调用 不需要主动调用next()函数
router.afterEach((to, from) => {
    console.log('----');
}) 
}
~~~

### 5.3 路由独享守卫

可以到官网学习 : 导航守卫

~~~js
{
    path: '/profile',
    component: Profile,
    meta: {
        title: '我的'
    },
        //只有进入我的 才会调用
    beforeEach((to, from, next) => {
    console.log('in mine');
    next(); //必须调用next  不然不会进行下一步
	})
}
~~~

### 5.4 组件内的守卫

~~~js
const foo ={
    template:``
    ...
}
~~~

## 6.vue-router-keep-alive

页面没有保存 组件之前的跳转  

比如：在默认展示新闻的首页，点击消息后，又跳转到 关于 的页面，再跳转会首页的时候，展示的还是新闻的页面

因为组件的生命周期：在跳转到关于页面的时候，把首页的组件 销毁了，然后在点击回首页的时候，是重新创建了一个首页组件 

不希望被创建新的时候，使用keep-alive

### 6.1 keep-alive 遇见 vue-router

+ keep-alive是Vue内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染
  + include - 字符串或正则表达式，只有匹配的组件会被缓存
  + exclude - 字符串或正则表达式，任何匹配的组件都不会被缓存
+ router-view是vue-router的一个组件，如果直接被包再keep-alive里面，所有路径匹配到的视图组件都会被缓存

### 6.2 保存组件状态的解决

> 在App.vue中，将`<router-view>`放在`<keep-alive>`便签里，就可以不重新创建组件
>
> ~~~vue
> <keep-alive><router-view/></keep-alive>  //保持组件状态
> ~~~



但是这样还是不行

+ 解决方法一： 还是不行`第二次回到页面的时候，就不行了`
  + 不在index.js下面设置缺省，`{  path: '',  redirect: 'news' }`
  + 在Home.vue的created生命周期里，添加`this.$router.push('/home/news')`
+ 解决方法二：还是不行`后保存的path值是后面点击到的活跃状态的path`
  + 在Home.vue的data保存一个路径`path:'/home/news'`
  + `activated(){  this.$router.push(this.path)  }`
  + `deactivated(){    this.path = this.$router.path;  }`
+ 解决方法三：行了 组件内导航
  + 在Home.vue：`activated(){  this.$router.push(this.path)  }`
  + `beforeRouteLeave(to,from,next){ this.path = this.$route.path; next() }`

 **activated() / deactivated()只有该组件被保持了状态 使用了keep-alive时才有效**

