## 在setup中获取之间实例对象

使用全局api`getCurrentInstance`可以获取到组件的实例对象（只能在生命周期钩子中使用）

```js
import {getCurrentInstance} from 'vue'

const {proxy} = getCurrentInstance()  // proxy 对象，除此之外还有ctx，是普通对象
```

获取挂载到全局的方法

```js
const instance = getCurrentInstance()
const _this = instance.appContext.config.globalProperties
	```


## 在setup中的await

在使用setup语法糖时，可以直接写`await`，会自动将整个`setup`变为`async`，需要注意的是，一定要在父组件使用`<Suspense>`包裹

## 获取DOM元素的ref

```vue
<template>
	<div ref="div"></div>
</template>

<script setup>
import {ref} from 'vue'
// 获取DOM元素
const div = ref()  // 注意变量名必须和 ref 设置的名字一致

// 使用
console.log(div.value)
</script>
```

## 获取router和route

```js
import { useRouter, useRoute } from 'vue-router'
const router = useRouter()
const route = useRoute()

// 使用
if (route.name === 'login') {
	router.push('/home')
}
```


## 获取store

```js
import { useStore } from 'vuex'
const store = useStore()

// 使用
store.commit('tab/changeCollapse')
```

## reactive声明的对象失去响应式

reactive声明对象是不能直接赋值一个新的对象，这样会失去响应式，如果不想失去响应式可以选择以下方案

### 1、ref

```js
import {ref} from 'vue'

const obj = ref({})

// 修改
obj.value = {}  // 直接赋值是不会失去响应式的
```

### 2、reactive

```js
import {reactive} from 'vue'

const obj1 = reactive({
	obj: {}  // 我们需要的对象
})

// 修改
obj1.obj = {}  // 不会失去响应式
```

如果都不想使用，可以选择使用循环来为每一个`key`更新值


## 动态路由

在`vite`中并不支持采用`(resolve) => require([""], resolve)`动态导入路由

在动态添加路由时，仅支持已`./`或`../`开头且以文件后缀结尾的路径，对于自定义的`@/`使用`() => import()`导入并不支持

对于动态路由在刷新时丢失，可能时添加路由的时机太靠后，可以尝试在`main.js`中添加路由


## Vue3中使用TS

在Vue3中使用TS两种写法

### 泛型约束

利用泛型对变量进行约束（代码量增加，不推荐）

```ts
import {reactive} from 'vue'

interface Person {
	name: string,
	age: number
}

const jack = reactive<Person> {
	name: "",
	age: 0
}
```

在`.vue`一般是不编写于业务无关的代码，例如类型约束，故可以在另外一个ts文件专门用于类型

在`src`下创建一个`type`目录，此目录下存放`.ts`文件，用于声明类型

```ts
// src/type/index.ts
interface IData {
    name: string,
    type: number
}

const jack = {
	name: 'jack'
}
  
export type {   // 导出 type 用于泛型约束
    IData
}

export {
	jack   // 导出数据
}

// 使用  .vue
import type {IData} from '@/type/index'   // 表示导入 type
// 通过泛型约束
```

### 实例

在`src`下创建`pageJS`目录，此目录用于上述的实现接口（常用）

```ts
// src/type/index.ts
interface IData {
    name: string,
    type: number
}
  
export type {   // 导出 type 用于泛型约束
    IData
}

// src/pageTS/index.ts
import {IData} from '../type/index'
  
class InitData implements IData {
    name: string = ''
    type: number = 0
}
  
export {
    InitData
}

// .vue  使用
import {InitData} from 'pageTS/index'

// 创建实例
const data = reactive(new InitData)
```