### 路由

#### 前端路由的概念与原理

路由的本质是对应关系。路由分为两类：
* 前端路由
* 后端路由
	请求方式、请求地址与 `function` 处理函数之间的对应关系


**SPA 与前端路由**

在 SPA 中，不同组件（功能）之间的切换需要依赖前端路由来实现

前端路由：Hash 地址与组件之间的对应关系
工作方式：
* 用户点击页面的路由链接
* URL 地址栏中的 Hash 值发生变化
* 前端路由监听到 Hash 地址的变化
* 前端路由把当前的 Hash 地址对应的组件渲染到浏览器中

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220622152623.png)

#### vue-router 的基本使用

vue-router 是 vue 官方给出的路由解决方案（插件）。只能结合 vue 项目使用（3.x版本适用于vue2.x，4.x版本适用于vue3.x）。路由的数据需要通过 `ajax` 获取。被切换走的路由组件默认是被销毁了

安装 `vue-router 4.x` 

```sh
npm i vue-router -S   # vue3

npm i vue-router@3 -S   # vue2
```

使用 `<router-link>` 标签声明路由链接，并使用 `<router-view>` 标签声明路由占位符

```vue
<!-- App.vue -->
<template>
  <div>
    <h1>App.vue</h1>
    <!-- 路由连接 -->
    <router-link to="/home">首页</router-link>;
    <router-link to="/movie">电影</router-link>;
    <router-link to="/about">关于</router-link>
    
    <!-- 路由占位符 -->
    <router-view></router-view>
  </div>
</template>
```

每个路由组件都有两个属性`$route`（只读对象）和`$router`（只写对象），前者只存放自己的`router`对象，而第二个存放所有参与路由的组件。可以在组件中使用`thi.$route`获取组件路由的配置信息

**配置路由**

在项目中创建 `router.js` 路由模块，在其中创建并得到路由对象
* 从 vue-router 中按需导入两个方法
* 导入需要使用路由控制的组件
* 创建路由实例对象
* 向外共享路由实例对象
* 在 `main.js` 中导入并挂载路由模块

```js
// createRouter 创建旅游的实例对象  
// createWebHashHistory 用于指定路由工作模式为 hash 模式  
import {createRouter, createWebHashHistory} from "vue-router"  
  
import Home from "./MyHome.vue"  
import Movie from "./MyMovie.vue"  
import About from "./MyAbout.vue"  
  
const routes = [  
    {path: '/home', component: Home},  
    {path: '/movie', component: Movie},  
    {path: '/about', component: About}  
]  
  
// 创建路由实例对象  
const router = createRouter({  
    // 指定工作模式  
    history: createWebHashHistory(),  
    // 通过 routes 数组，指定路由规则  
    routes  
});  
  
export default router


// main.js
import App from './App.vue'
import router from './components/router/router.js'
  
const app = createApp(App)
// 挂载路由模块
app.use(router);
```

vue2路由器使用示例

**router.js**
```js
import VueRouter from "vue-router";  
  
import Home from "@/components/Home";  
import About from "@/components/About";  
  
const routes = [  
    {  
        path: "/home",  
        component: Home  
    },  
    {  
        path: "/about",  
        component: About  
    }  
]  
  
export default new VueRouter({  
    routes  
})
```

**main.js**
```js
import Vue from 'vue'  
import App from '@/App.vue'  
import router from "@/router/route";  
import VueRouter from "vue-router";  
  
Vue.use(VueRouter)  
  
Vue.config.productionTip = false  
  
new Vue({  
    render: h => h(App),  
    router  
}).$mount('#app')
```

**App.vue**
```vue
<template>  
  <div>    <!-- 通过 active-class 指定被选中时的样式 -->
    <router-link active-class="active" to="/about">About</router-link>  
    <br>  
    <router-link active-class="active" to="/home">Home</router-link>  
  
    <router-view></router-view>  
  </div>  
</template>  
  
<script>  
  
export default {  
  name: "App",  
  
}  
</script>  
  
<style scoped>  
.active {  
  background-color: red;  
  color: white;  
  font-weight: bold;  
}  
</style>
```


#### vue-router 的高级用法

**重定向**

路由重定向：用户在访问地址 A 的时候，强制用户跳转到地址 C，从而展现特定的组件页面

通过路由规则的 `redirect` 属性，指定一个新的路由地址，设置路由的重定向

```js
const routes = [  
    {path: '/', redirect: '/home'},  
    {path: '/home', component: Home},  
    {path: '/movie', component: Movie},  
    {path: '/about', component: About}  
]
```

**路由高亮**

通过两种方案实现对激活的路由进行高亮显示
* 使用默认的高亮class类
	被激活的路由链接，会默认引用在 `index.css` 中的一个 `router-link-active` 的类名

	```css
	/* 自定义 */
	.router-link-active {  
	    background-color: red;  
	    color: white;  
	    font-weight: bold;  
	}
	```
	
* 自定义路由高亮的class类
	在创建路由实例对象时，可以基于 `linkActiveClass` 属性，自定义路由连接被激活时应用的类名
	也可以在`route-link`标签中使用`active-class`属性指定样式

	```js
	const router = createRouter({  
	    // 指定工作模式  
	    history: createWebHashHistory(),  
	    // 通过 routes 数组，指定路由规则  
	    routes,  
	    // 指定路由被激活时应用的样式的类名  
	    linkActiveClass: 'router-active'  
	});
	```

**嵌套路由**

嵌套路由：通过路由实现组件的嵌套展示
* 声明子路由连接和子路由占位符

	```vue
	<!-- 在about中声明两个子路由 -->
	<template>  
	  <div>  
	    <p>MyAbout.vue</p>  
	    <router-link to="/about/tab1">tab1</router-link>  
	    <router-link to="/about/tab2">tab2</router-link>  
	      
	    <router-view></router-view>  
	  </div>  
	</template>
	```

* 在父路由的规则中，通过 `children` 属性嵌套声明子路由规则

	```js
	// router.js
	const routes = [  
	    {path: '/', redirect: '/home'},  
	    {path: '/home', component: Home},  
	    {path: '/movie', component: Movie},  
	    {path: '/about', component: About,   
	        children: [    // 子路由不要以 / 开头，底层遍历时自己加 / 
	            {path: 'tab1', component: Tab1},  
	            {path: 'tab2', component: Tab2}  
	        ]}  
	]
	```

在嵌套路由中使用路由的重定向

```js
// 从 /about 重定向到 /about/tab1
const routes = [    
    {  
        path: '/about',  
        component: About,  
        redirect: "/about/tab1",  
        children: [  
            {path: 'tab1', component: Tab1},  
            {path: 'tab2', component: Tab2}  
        ]  
    }  
]
```

##### 路由参数

**query参数**

传参可以通过v-bind和模板字符串的形式传递（同ajax中的query），或使用对象的形式。由组件中可以通过`this.$route.query`对象获取传入的参数

```vue
<!--Table.vue-->
<template>  
  <div>  
    <h1>Table</h1>  
    <ul>  
      <li v-for="p in list" :key="p.id">  
        <!-- 借助v-bind和模板字符串传参 -->
        <router-link :to="`/home/table/detail?id=${p.id}&title=${p.title}`">详情</router-link>  
        <!-- 对象型式传参 -->
        <router-link :to="{  
		  path: '/home/table/detail',  
		  query: {  
		    id: p.id,  
		    title: p.title  
		  }  
		}">详情</router-link>
      </li>  
    </ul>  
    <router-view></router-view>  
  </div>  
</template>  
  
<script>  
export default {  
  name: "Table",  
  data: () => {  
    return {  
      list: [  
        {id: 1, title: "1"},  
        {id: 2, title: "2"},  
        {id: 3, title: "3"},  
        {id: 4, title: "4"},  
      ]  
    }  
  }  
}  
</script>  


<!--Detail.vue-->
<template>  
  <!--获取参数-->
  <h1>{{$route.query.id}}--{{$route.query.title}}</h1>  
</template>  
  
<script>  
export default {  
  name: "Detail",  
}  
</script>  
  
<style scoped>  
  
</style>
```

**命名路由**

通过 name 属性为路由规则定义名称（name必须保证唯一性），必须使用对象的形式来使用Name

```js
{  
    path: '/movie/:id',   
    name: "mov",  
    component: Movie,   
{
```

让路由连接动态绑定对象，对象中的 `name` 属性就是要指定跳转的路由（避免路径过长，可以只在对象中写一个name指定），同时对象中还可以使用 `params` 指定携带的参数（通过命名路由实现声明式导航）


**params参数**

**使用`params`传参只能使用`name`引入路由的方式，如果是path，那么解析后会是undefined**，而且不会再url中显示参数，相比之下`query`传参会显示参数

- 1、先在路由配置项中声明参数占位符

```js
{  
    name: 'de',  
    path: 'detail/:id/:title',  // 声明占位符
    component: Detail,  
}
```

- 2、传参

```vue
<router-link :to="{ 
	name: 'mov', 
	params: { id: 3 } 
}">go to movie</router-link>
```

在使用 `this.$router.push()` 方法时，可以使用对象的方式指定

```js
this.$router.push({ name: 'mov', params: { id: 3 } });
```

**动态路由匹配**

动态路由：将 Hash 地址中可变的部分定义为参数项，从而提高路由规则的复用性（使用 `:` 来定义路由参数项）

```js
// 路由链接
<router-link to="/movie/1">电影</router-link>  
<router-link to="/movie/2">电影</router-link>  
<router-link to="/movie/3">电影</router-link>

// 路由规则

// id 为动态参数
{ path: 'mocie/:id', componenet: Movie }


// 可以等效于
{ path: 'mocie/1', componenet: Movie }
{ path: 'mocie/2', componenet: Movie }
{ path: 'mocie/3', componenet: Movie }
```

可以使用 `$route.params.参数名` 访问参数，也可以使用 `props` 接收路由参数

- 1、在配置对象中开启`props:true`**此形式会将params参数以props形式传递**（不会管query参数）

```vue
<!-- router.js 允许props传参 -->
{path: '/movie/:id', component: Movie, props: true}

<!-- Movie.vue -->
<p>通过属性访问 {{$route.params.id}}</p>  
<p>通过props访问 {{id}}</p>

<script>  
export default {  
    name: "MyMovie",  
    props: ['id']  
};  
</script>
```

- 2、还可以将对象以 `props` 的形式传递参数

```js
{  
    name: 'de',  
    path: 'detail/:id/:title',  
    component: Detail,  
    props: {
	    a: 1,
	    b: 2
    }  
}
```

- 3、还可以将props写为函数，函数会传入参数`$route`在这里可以使用解构赋值形式直接传入`query`或`parmas`，**函数必须有返回值而且返回值必须是对象**，此对象会将数据以`props`传递参数

```js
{  
    name: 'de',  
    path: 'detail/:id/:title',  
    component: Detail,  
    props({query:{id, title}) {   // 解构赋值连续写法
	    return {
		    id: id,
		    title: title
	    }
    }
}
```


##### 编程式导航


编程式导航：通过调用 API 实现导航的方式（调用 `location.href` 实现跳转页面）
声明式导航：通过点击连接实现导航的方式（ `<a>` 标签或者 vue 项目中的 `<router-link>` ）

`vue-router` 提供的常用的编程式导航 API（都在vue-router的原型对象上）：
* `this.$router.push({})`
	* 跳转到指定配置对象或url，配置对象的配置和声明式戴航配置相同（向浏览器历史记录栈中添加）
	* 浏览器中会以栈的形式保存历史记录，每一次的url变化，都会保存到栈中
	* 当点击浏览器的后退时指向栈顶元素的指针向下直到栈底元素，当前进时指针向上直到栈顶元素
* `this.$router.replace({})`
	* 通过 `replace` 模式跳转
* `this.$router.go(数值 n)`
	* n 可以是正数也可以是负数，实现导航的前进和后退
* `this.$router.back()`
	* 后退一次
* `this.$router.forward()`
	* 前进一次


浏览器历史记录的写入模式除了 `push` 模式还有 `replace` 模式，此模式会**替换掉栈顶元素**，不同于 `push` 元素的不破坏元素（默认为`push`模式）

如果想开启`replace`模式，需要在`router-link`标签中添加属性`replace或:replace="true"`即可


```vue
<!-- 点击按钮，跳转到 Movie -->
<script>  
export default {  
  name: "MyHome",  
  methods: {  
    goToMovie(id) {  
      // ('/movie/' + id)
      this.$router.push(`/movie/${id}`)  
    }  
  }  
}  
</script>


<!-- 点击按钮 回退上一个组件页面 -->
<script>
export default {  
  name: "MyMovie",  
  props: ['id'],  
  methods: {  
    goBack() {  
      this.$router.go(-1)  
    }  
  }  
};
</script>
```


##### 缓存路由组件

由于当切换路由后，路由是默认会被销毁的，所有之前输入的表单信息等，都会被销毁

如果需要保存这些内容，需要在组件的存放位置（父组件）中使用`keep-alive`标签包裹

```vue
<keep-alive include="Home">
  <router-view></router-view>  
</keep-alive>
```

由于此标签包裹下的所有的路由都不会被销毁，如果需要指定路由缓存，那么就需要**配置`include`属性指定组件名**，多个使用数组结合`v-bind`指令

**路由中独有的生命周期钩子**

由于缓存路由组件后，该路由不会被销毁，那么在销毁的生命周期钩子中的销毁定时器、解绑事件等操作都不会执行，导致效率低下

路由中独有两个生命周期钩子
* `activated`：路由被激活
* `deactivated`：路由失活

这两个声明周期只有在`router-view`被`keep-alive`包裹时（缓存）会被调用

##### 路由守卫

路由守卫可以控制路由的访问权限

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220623021738.png)

全局导航守卫：会拦截每个路由规则，对每个路由规则进行访问权限的控制

###### 全局守卫

**全局前置路由守卫**

初始化被调用，每次路由切换之前调用

```js
// router.js

const router =  new VueRouter({  
    routes  
})

router.beforeEach(() => {  
    console.log("触发守卫方法")
})
```

守卫方法的三个形参
* `to`：目标路由对象
* `from`：当前导航正要离开的路由对象
* `next`：函数，表示放行

```js
// router.js
router.beforeEach((to, from, next) => {  
    console.log(to)  
    console.log(from)  
    // 如果声明了 next 必须调用 next 负责不允许访问任何组件  
    next();  // 允许访问任何组件  
})
```

`next` 函数的三种调用方式

直接放行：`next()`
强制停留当前页面：`next(false)`
强制跳转到指定页面：`next('hash地址')`

结合 `token` 控制页面得访问权限

```js
router.beforeEach((to, from, next) => {  
    const token = localStorage.getItem('token')  // 读取 token    
    if (to.path === '/main' && !token) {  
        next('/login')   // 登录
    } else {  
        next()    // 放行
    }  
})
```

在路由守卫中，可以将需要校验的路由的`meta`属性（路由元信息）上存放标识例如`meta: {isLogin: false}`，这样在校验时，可以直接通过唯一标识，来判断是否需要校验。`meta`属性是一个对象，用于存放自定义的属性

**全局后置路由守卫**

初始化被调用，每次路由切换之后调用

后置路由守卫没有`next`

可以用于切换后更改标签页的 title

```js
router.afterEach((to, from) => {  
    document.title = to.meta.title
})
```

###### 独享守卫

只想对单独一个路由编写，在路由的配置对象中编辑
* `beforeEnter: (to, from, next) {}`：进入页面之前

**独享路由守卫只有前置没有后置**

如果同时配置了同一个路由的守卫，那么以全局为准

###### 组件内守卫

在组件中的配置对象中有配置项
* `beforeRouteEnter: (to, from. next) => {}`
	* **通过路由规则**，进入该组件时被调用
* `beforeRouteLeave: (to, from. next) => {}`
	* **通过路由规则**，离开该组件时被调用

注意此时不论是箭头函数还是普通函数，this都是指向的  `undefined`

#### 路由器的两种工作模式

上述的案例中，在url都会显示`/#/`这个`#`称为 `hash` 以`#`开始到后面都称为路径中的`hash`值，不会随着`http`请求给服务器，请求的地址不会计算`hash`值得那一部分

路由器默认开启`hash`工作模式，还有一个`history`工作模式，需要在路由配置对象中通过`mode`配置项来配置

```js
const router =  new VueRouter({  
    mode: 'history',    // mode: 'hash'
    routes  
})
```

使用`history`工作模式路径直接是`/`，但是兼容性没有`hash`好

项目写完后，需要进行打包`npm run build`，会在根目录生成文件夹`dist`，打包后得文件必须在服务器上**部署**才能使用

`history`模式在上线后（前后端不分离），由于前端路由不会发起网路请求，当刷新后会将url当作资源访问，此时后端没有此资源，会导致`404`。需要后端匹配判断是前端还是后端的路由。在`nodejs`中有第三方包`connect-history-api-fallback`解决

```js
// server.js
const express = require('express')
const history = require('connect-history-api-fallback')

const app = express()
app.use(history())
```
