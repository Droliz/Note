### vue 综合

#### vue-cli

**简介**

vue-cli （vue脚手架）是 vue 官方提供得、快速生成 vue 工程化项目的工具

`vue-cli` 基于webpack，支持vue2和vue3项目

**安装和使用**

```sh
# 全局安装
npm i -g @vue/cli

# 查看vue-cli版本
vue --version

# 查看版本不识别 vue 
# 管理员运行cmd
set-ExecutionPolicy RemoteSigned
Y   # 回车
```

创建一个 vue-cli 项目

```sh
vue create project_name   # 以命令行方式创建

vue ui   # 以 ui 界面的形式创建

# 启动项目
npm run server
# 打包项目（编译并压缩）
npm run bulid
```


```sh
# 如果在创建项目是终端报错
 ERROR  Failed to get response from Error: JAVA_HOME is incorrectly set.
	Please update XXXX

# 这是因为安装的 haddop 等其他的中的 yarn 和 npm 冲突

# 解决方案：在 C:\Users\lenovo\vuerc（vue的配置文件）中修改如下
{
  "packageManager": "npm",
  "useTaobaoRegistry": false,
}
```

**vue-cli项目目录**

使用 treer 生成目录树形结构

```sh
npm i -g treer 
treer -i "/node_modules|.git/"   # 多个可以使用正则表达式
```

```txt
├─babel.config.js     // 将 ES6 语法转为 ES5，需要相关配置
├─jsconfig.json       // 
├─package-lock.json   // 包管理
├─package.json        // 包管理
├─README.md           // 项目文档
├─vue.config.js       // vue设置
├─src                 // 存放代码
|  ├─App.vue          // 根组件
|  ├─main.js          // 入口文件（全局配置、vm等）
|  ├─components       // 放置组件
|  |     ├─HelloWorld.vue
|  |     └School.vue
|  ├─assets           // 静态资源
|  |   └logo.png
├─public              // 页面
|   ├─favicon.ico
|   └index.html       // 主页面
```

`main.js`中的`render`配置项

render是一个函数，当引入残缺版vue（没有模板解析），而需要写template配置时，可以使用render

```js
// main.js
new Vue({
	el: '#app',
	render(createElement) {
		return createElement('h1', 'msg')   // 创建元素，可以直接写vue组件
	}
	// 简写 render: h => h('h1', 'msg')
})
```

脚手架的配置（主页面，入口文件等）在`vue.config.js` 中配置

配置关闭语法检查（当文件有变量未使用，都会导致无法启动）

```js
const { defineConfig } = require('@vue/cli-service')  
module.exports = defineConfig({  
  transpileDependencies: true,  
  lintOnSave: false,    // 关闭语法检查  
})
```



#### 组件库

**简介**

vue组件库：前端开发者把自己封装的组件整理、打包、并发布为 npm 的包，从而供其他开发者使用
* bootstrap 只提供纯粹的原样式、结构、特效等，需要开发者自己进一步的组装和改造
* vue 组件库是遵循 vue 语法、高度定制的现成的组件

**常用的 vue 组件库**

pc端
* [Element UI](https://element.eleme.cn/#/zh-CN)
* [View UI](http://v1.iviewui.com/)

移动端
* [Mint UI](http://mint-ui.github.io/#!/zh-cn)
* [Vant](https://vant-contrib.gitee.io/vant/#/zh-CN)

Element UI：是饿了么前端团队开源的一套 PC 端的 vue 组件库。支持 vue2.x 和 vue3.x

下载依赖（-plus是vue3的，-ui是vue2的）

```sh
npm i element-plus -S
```

注册 element-ui 为 vue 插件

```js
// main.js
import { createApp } from 'vue'  
import App from './App.vue'  
// 导入 elment-plus
import ElementPlus from 'element-plus';  
// 导入全局样式  
import 'element-plus/lib/theme-chalk/index.css';  
const app = createApp(App)  
// 注册为插件  
app.use(ElementPlus)  
app.mount('#app')
```

按需引入需要借助第三方库`babel-plugin-component`

```sh
npm i babel-plugin-component -D
```

配置`babel.config.js`

```js
module.exports = {  
  presets: [  
    '@vue/cli-plugin-babel/preset',  
    ["@babel/preset-env", { "modules": false }],  
  ],  
  plugins: [  
    [  
      "component",  
      {  
        "libraryName": "element-ui",  
        "styleLibraryName": "theme-chalk"  
      }  
    ]  
  ]  
}
```

部分导入

```js
import { Button, Select } from 'element-ui';  
  
Vue.component(Button.name, Button);  
Vue.component(Select.name, Select);  
// Vue.use(Button)  
// Vue.use(Select)
```

详情参考[官方文档](https://element.eleme.cn/#/zh-CN/component/quickstart)

#### axios 拦截器

**概念**
在每次发起 ajax 请求和得到响应的时候自动被触发

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220623232850.png)

应用于：token身份验证、loading 效果 等

**配置请求拦截器**

通过调用 `axios.interceptors.request.use(成功回调，失败回调)` 失败回调可省略。成功回调可以接收 config 对象，而且必须将 config 返回，否则请求无法发起

```js
// request.js
import axios from 'axios'
import store from '@/store'
import router from '@/router'
 
export const baseURL = 'http://www.xxx.com'
const service = axios.create({
    baseURL,
    timeout: 5000
})
 
// 请求头拦截器
service.interceptors.request.use(config => {
    // 拦截业务逻辑，内容可替换
    // 1. 获取用户信息对象
    const { profile } = store.state.user;
    // 2. 判断是否有token
    if (profile.token) {
        // 3. 设置token
        config.headers.Authorization = `Bearer ${profile.token}`
    }
    return config
}, err => {
    return Promise.reject(err);
})
 
// 响应拦截器
service.interceptors.response.use(res => res.data, err => {
    // 失败时进入这里，内容根据store文件里的内容可替换
    if (err.response && err.response.status === 401) {
        // 清空无效用户信息
        store.commit('user/setUser', {});
        // encodeURIComponent转换uri编码，防止解析地址出问题
        // router.currentRoute.value.fullPath就是当前路由地址
        const fullPath = encodeURIComponent(router.currentRoute.value.fullPath);
        // 跳转到登录页
        router.push('login/redirectUrl=' + fullPath)
    }
    return Promise.reject(err)
})
 
export default (url, method, submitData) => {
    return service ({
        url,
        method,
        // 写个三元来转换
        [method.toLowCase() === 'get' ? 'params' : 'data']: submitData
    })
}
```

#### proxy 跨域代理

在项目遇到接口跨域的问题时，可以通过代理的方式解决
* 把 `axios` 请求的根目录设置为 vue 项目的运行地址（接口请求不在跨域）
* vue 项目发现请求的接口不存在，就会将请求转交给 `proxy` 代理
* 代理把请求的根路径替换为 `devServer.proxy` 属性的值，发起真正的数据请求
* 代理将请求过来的数据，转发给 axios

在项目根目录创建配置文件 `vue.config.js` 然后添加如下配置。重启项目即可

```js
// 项目根目录 vue.config.js
module.exports = {
    devServer: {
	    // api 接口地址
        proxy: "http://www.api.droliz.cn",
    }
}


// main.js
// 当前服务运行地址
axios.defaults.baseURL = 'http://localhost:3000'

// 发起请求
axios.get('/api/url/nav/list').then(  
    res => {  
      console.log(res.data);  
    },  
    err => {  
      console.log(err.message);  
    }  
)
```

上述的配置只能配置一种代理

添加完整的配置对象

```js
// vue.config.js
proxy: {  
    '/api': {   // 路径前缀为  /api 就代理   可以有多个配置项  
        target: "http://localhost:5000",   // 跨域的目标域名  
        ws: true,    // 用于支持 websocket
        changeOrigin: true,   // 让后端认为该请求是同源的（相同域名端口）  默认为 true
		pathRewrite: {      // 重写路径  
            "^/api": "/api"    
        }  
    }  
}
```


