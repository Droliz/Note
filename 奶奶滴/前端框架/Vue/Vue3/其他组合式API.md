# 其他组合式API

## shallowReactive与shallowRef

`shallowReactive`只处理对象最外层属性的响应式（浅响应式）

`shallowRef`只处理基本数据类型的响应式，不处理对象类型

如果对象数据结构深，但是变化的只有最外层，使用`shallowReactive`

如果一个对象数据后续功能**不会修改该对象中的属性**，而是生成新的对象来替换，使用`shallowRef`

## readonly与shallowreadonly

让响应式数据只读

## toRaw与markRaw

- `toRaw`
	- 将一个`reactive`生成的响应式对象转为普通对象
- `markRaw`
	- 标记一个对象，使其永远不能称为响应式对象

## customRef

创建一个自定义的ref，并对其依赖项跟踪和更新触发进行显示控制

custimRef必须接收一个函数，接收两个参数（`track`、`trigger`），函数返回一个对象，对象中必须有`getter、setter`

- `track`：追踪（在get返回之前调用）
- `trigger`：函数去通知 vue 重新解析模板

当数据改变，vue重新解析模板，会找get要数据，此时get需要提前通过vue追踪value的变化，否则不会生效

简单使用

```vue
<template>  
  <div>  
    <input type="text" v-model="keyWord"/>  
    <h3>{{ keyWord }}</h3>  
  </div>  
</template>  
  
<script>  
  
import {customRef} from "vue";  
  
export default {  
  name: "ComponentTest",  
  // 无响应式  
  setup() {  
    // 自定义 ref    function myRef(value) {  
      return customRef((track, trigger) => {  
        return {  
          get() {  
            console.log('get', value)  
            track()  
            return value  
          },  
          set(val) {  
            console.log('set', value, val)  
            value = val  
            trigger()  
          }  
        }  
      })  
    }  
  
    let keyWord = myRef('')  
  
    return {  
      keyWord  
    }  
  }  
}  
  
</script>  
  
<style scoped>  
  
</style>
```


实现防抖效果：可以让`trigger`或`track`等待执行

```js
function myRef(value, delay) {  
  let timer  
  return customRef((track, trigger) => {  
    return {  
      get() {  
        track()  
        return value  
      },  
      set(val) {  
        clearTimeout(timer)  
        timer = setTimeout(() => {  
          value = val  
          trigger()  
        }, delay)  
      }  
    }  
  })  
}
```


## provide与inject

实现**父与后代组件**中的通信

祖组件通过`provide`提供数据，子组件通过`inject`接收使用数据

祖组件

```js
import {provide} from "vue"

setup() {
	let car = reactive(
		{
			name: 'bc',
			price: 48
		}
	)

	provide("car", car)
}
```

孙组件

```js
import {inject} from 'vue'

const car = inject('car')
```


## 响应式数据的判断

- isRef：检查是否为 ref 对象
- isReactive：检查是否由`reactive`构建的响应式代理
- isReadonly：检查是否由`readonly`构建的只读代理
- isProxy：检查是否由`reactive`、`readonly`构建的代理

