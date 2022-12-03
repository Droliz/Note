# vue后台管理系统

## 技术栈

- vue-router
- vuex
- axios
- element-ui
- mock     模拟后端接口
- echarts     前端可视化、
- less


## yarn与npm

yarn适用于弥补npm的缺陷（慢，版本不统一）

yarn采用并行安装，提高性能，而且yarn的离线模式使得只要安装一次就会在缓存中保存

yarn安装版本统一，每次生成`lock`文件，会锁定模块版本


## 引入element-ui

安装

```sh
npm i element-ui -S
```

全局引入

```js
// main.js
import elementUI from "element-ui"
import "element-ui/lib/theme-chalk/index.css"

Vue.use(elementUI)
```

按需映入

```js
// babel.config.js

module.exports = {
  presets: [
    '@vue/cli-plugin-babel/preset',
    ["es2015", { "modules": false }]   // 如果显示没有es2015，改为 @babel/preset-env
  ],
  "plugins": [
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


```js
// main.js
import { Row, Button } from "element-ui"
import "element-ui/lib/theme-chalk/index.css"

Vue.use(Row)
Vue.use(Button)
```

## vue-router

安装（注意vue2.x使用的版本是3.x，vue3.x使用的版本是4.x）

```sh
npm i vue-router -S
```

配置

在`src`下创建文件夹`router`，创建文件`index.js`

```js
// @/router/index.js
import Vue from "vue";  
import VueRouter from "vue-router";  
  
Vue.use(VueRouter)  
  
const routes = [  
    {  
        path: "/",  
        component: () => import("@/views/Main"),  
        children: [  
            {  
                path: "home",  
                component: () => import("@/views/Home")  
            },  
            {  
                path: "user",  
                component: () => import("@/views/User")  
            }  
        ]  
    },  
  
]  
  
const router = new VueRouter({  
    routes  
})  
  
export default router
```


## less

下载

```sh
npm i less@4.1.2
npm i less-loader@6.0.0
```


## 