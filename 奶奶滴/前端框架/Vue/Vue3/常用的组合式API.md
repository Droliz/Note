# 常用组合式API

## setup

**Vue3.0**中的一个新的配置项，值为一个函数，**setup**是所有组合式API的 “表演舞台”

组件中所用到的：数据、方法等等（生命周期钩子....）写在**setup**中

### 基本使用

```js
// App.vue
export default {  
  name: "App",  
  // 无响应式  
  setup() {  
    let count = 1  
    let number = 0  
  
    function func() {  
      alert('func')  
      console.log(`count: ${count}`)  
      console.log(`number: ${number}`)  
    }  
  
    return {  
      count,  
      number,  
      func  
    }  
  }  
}
```

除了上述的返回对象，**setup**还可以返回一个渲染函数

如果返回的是对象，在模板中可以直接使用

```html
<template>  
  <div>  
    <p>count: {{ count }}</p>  
    <p>number: {{ number }}</p>  
    <button @click="func"></button>  
  </div>  
<template>
```

返回渲染函数`h()`，模板中的内容会按照渲染函数的结果为主（模板内容不会显示）

```js
import {h} from 'vue'  
  
export default {  
  name: "App",  
  // 无响应式  
  setup() {  
    let count = 1  
    let number = 0  
  
    function func() {  
      alert('func')  
      console.log(`count: ${count}`)  
      console.log(`number: ${number}`)  
    }  
  
    return () => h('h1', count)  
  }  
}
```

### setup注意点

setup在`beforeCreate`之前执行一次，`this`是`undefined`

setup参数
- props：值为对象，包含组件外部传递过来，且在props配置中声明的属性
- context：上下文对象
	- attrs：对象，包含组件外部传递，但是没有在props配置中声明的属性，`this.$attrs`
	- slots：收到的插槽内容，`this.$slots`
	- emit：自定义函数，`this.$emits`


**props是一个响应式的proxy对象，因此不能解构，否则会失去响应式**（vue3中自定义事件需要在子组件使用`emits`配置项声明，类似`props`配置项）

- 通过vue2的方法的配置/方法（data、methods....），是可以读取通过vue3的**setup**配置的数据和方法。
- vue3的**setup**中是不能访问到vue2配置的方法和数据（不混用）
- 如果数据或方法名相同，那么vue3为主
- **setup**不能是一个**async**函数（返回值是promise，不再是对象），模板无法访问返回对象的属性（使用`suspense`可以解决）


## ref函数

**ref函数**与ref是不同的，ref是用于操作DOM，而**ref函数**是用于创建响应式数据的

### 基本使用

声明响应式数据

```js
import {ref} from 'vue'  
  
export default {  
  name: "App",  
  // 无响应式  
  setup() {  
    const count = ref(1)    // 声明响应式的数据
    const number = ref(0)   
  
    function func() {  
      console.log(count)  
      console.log(number)  
    }  
  
    return {  
      count,  
      number,  
      func  
    }  
  }  
}
```

通过**ref函数**声明的响应式数据（深层数据也是响应式的），不单是一个数据，而是一个`RefImpl`实例对象（reference implement）引用实现对象

![](../../../../markdown_img/Pasted%20image%2020221106131316.png)


此处的**value**也是通过getter和setter来实现数据劫持，此时的原型上就相当于vue2的`_data`

vue3模板在解析时会自动读取value，但是如果想要修改需要读取value

```js
setup() {  
  const count = ref(1)  
  const number = ref(0)  
  
  function func() {  
    count.value = 2  
    number.value = 3  
    console.log(count)  
    console.log(number)  
  }  
  
  return {  
    count,  
    number,  
    func  
  }  
}
```

对于基本数据类型：响应式依靠`get、set、Object.degineProperty()`实现

对于对象：内部借助Vue3的新函数`reactive`

## reactive函数

用于定义一个对象类型的响应式数据（基本类型使用`ref`），返回一个代理器对象`proxy Object`

reactive定义的响应式数据是深层次的，内部基于`ES6`的`Proxy`实现，通过代理对象操作元对象内部数据都是相应的

### 基本使用

```js
setup() {  
  const arr = reactive({  
    name: 'zs',  
    age: 18  
  })  
  
  function func() {  
    console.log(arr)  
    arr.age = 80    // 修改数据
    arr.name = 'ls'  
  }  
  
  return {  
    arr,  
    func  
  }  
}
```



## Vue3响应式原理

### Vue2响应式

原理：
- 对象类型： 通过`Object.defineProperty()`对属性的读取、修改进行拦截（数据劫持）
- 数组类型：通过重写更新数组的一系列方法类实现拦截（push等）

新增属性、删除属性，界面不会更新。不能通过索引修改元素

```js
// 源数据
let person = {  
    name: 'zs',  
    age: 18  
}  
  
let p = {}  // 响应数据
for (let key of Object.keys(person)) {   // 对每一个 key 数据劫持  
    Object.defineProperty(p, key, {  
        get() {  
            return person[key]  
        },  
        set(val) {  
            person[key] = val  
        }  
    })  
}  
console.log(p.age)
```

### Vue3响应式

原理：
- 通过Proxy（代理）：拦截对象（源对象）中任意属性的变化，包括：属性值的读写、属性的增删（Proxy是window自带的方法）
- 通过Reflect（反射）：对源对象的属性进行操作（window内置对象）

[MDN描述Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)
[MDN描述Reflect](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect)

**Proxy**

```js
let person = {  
    name: 'zs',  
    age: 18  
}  

// 代理
const p = new Proxy(person, {  
    get(target, propName) {  // 获取调用   target：源数据
        return target[propName]  
    },  
    set(target, propName, val) {  // 修改、增加调用
       target[propName] = val  
    },  
    deleteProperty(target, p) {  // 删除调用  
        return delete target[p]  
    }  
})  
  
console.log(p)  
p.name = 'ls'  
p.sex = '男'  
delete p.age  
console.log(person)
```

**Reflect**

通过`Reflect`操作会返回一个布尔值来判断操作是否成功

**Object与Reflect的defineProperty**

```js
let obj = {  
    a: 1,  
    b: 2  
}

Object.defineProperty(obj, 'c', {  
    get() {  
       return 3  
    }  
})  
Object.defineProperty(obj, 'c', {    // error：Object的defineProperty添加重复属性会报错
    get() {  
       return 4  
    }  
})

const x1 = Reflect.defineProperty(obj, 'c', {    // x1 = true
    get() {  
       return 3  
    }  
}) 

const x2 = Reflect.defineProperty(obj, 'c', {    // x2 = false
    get() {  
       return 4  
    }  
})
```


## 计算属性与监视


### 计算属性

计算属性采用`computed()`函数实现

```js
import {computed} from "vue";
setup(props, content) {  
  let person = reactive({  
    firstName: '张',  
    lastName: '三',  
    fullName: computed(() => {  
      return person.firstName + "-" + person.lastName  
    })
  })
  
  return {  
    person  
  }  
}
```

如果需要编写setter，那么使用对象形式传入`getter、setter`

## 监视

采用`watch()`函数实现

```js
import {watch} from "vue";

setup() {  
  let num = ref(0)  
  let msg = ref('msg')  
  
  watch([num, msg], (newval, oldval) => {  // 监听单个不需要数组
    console.log(newval, oldval)  
  }, {immediate: true})  // 监听配置对象
  
  return {  
    num,  
    msg  
  }  
}
```

如果是监视`reactive`定义的响应式数据，获取到的`oldval`还是`newval`，无法获取到`oldval`（目前没有解决方法）

不论层次多深，只要使用`reactive`定义，那么此数据就可以深度监视（**不包括此对象的属性**），即便在配置对象中关闭深度监视也无效

如果是监视对象中的单个属性（基本类型），那么需要使用 `() => obj.属性` 的形式，此时可以获取到正确的`oldval`

```js
setup() {
	let person = reactive({  
	  name: 'zs',  
	  age: 18  
	})  
	  
	watch(() => person.name, (newval, oldval) => {  
	  console.log(newval, oldval)  
	})  
	  
	return {  
	  person  
	}
}
```

在上述的对象中，如果监视的属性是一个对象（`reactive`上的一个属性），那么此时需要开启`deep`才能检测到

```js
setup() {
	let person = reactive({  
	  job: {
		  name: 'web',
		  time: '996'
	  }
	})  
	  
	watch(() => person.job, (newval, oldval) => {  
	  console.log(newval, oldval)  
	}, {deep: true})   // 需要开启deep否则监测不到 
	  
	return {  
	  person  
	}
}
```

[详情查看说明文档](https://cn.vuejs.org/api/reactivity-core.html)

## watchEffect函数

watch函数是即需要指明监视的属性，也要指明回调函数

**watchRffect**不用指明监视那个属性，监视的回调中用到那个属性，就代表监视那个属性（有点类似计算属性）

```js
setup() {  
  let num = ref(0)  
  let msg = ref('msg')  
  let person = reactive({  
    name: 'zs',  
    age: 18  
  })  
  
  watchEffect(() => {    // 只要 person.name 和 num 改变，就触发
    const name = person.name   
    const n = num.value  
    console.log(`name:${name}`)  
    console.log(`n:${n}`)  
  })  
  
  return {  
    num,  
    msg,  
    person  
  }  
}
```

## 生命周期

![](https://cn.vuejs.org/assets/lifecycle.16e4c08e.png)

只有卸载的名字改变了，其他没有改变

除了像vue2一样采用选项式API，vue3提供组合式API写法，关系如下
- `beforeCreate` ----> `setup`
- `created` ----> `setup`
- `beforeMount` ----> `onBeforeMount`
- `mounted` ----> `onMounted`
- `beforeUpdate` ----> `onBeforeUpdate`
- `Updated` ----> `onUpdated`
- `beforeUnmount` ----> `onBeforeUnmount`
- `unmounted` ----> `onUnmounted`

都可写在`setup()`中，接受一个回调


## 自定义hook函数

hook本质是一个函数，将setup中使用的组合式API进行封装，类似vue2中的混合（mixin）

自定义hook可以复用代码，让setup中的逻辑更清楚易懂（将与一个功能有关的数据、函数、生命周期钩子，全部封装起来）

在`src`下创建`hooks`文件夹，文件夹下存放自定义`hook`，在这些`js`文件中编写某个功能，然后引入

```js
// hooks/usePoint.js
import {onBeforeUnmount, onMounted, reactive} from "vue";  
  
export default function () {  
    let point = reactive({  
        x: 0,  
        y: 0  
    })  
  
    function savePoint(event) {  
        point.x = event.pageX  
        point.y = event.pageY  
        console.log(event.pageX, event.pageY)  
    }  
  
    onMounted(() => {  
        window.addEventListener('click', savePoint)  
    })  
  
    onBeforeUnmount(() => {  
        window.removeEventListener('click', savePoint)  
    })  
  
    return point  
}
```

使用

```js
import userPoint from "../hooks/userPoint";  
  
export default {  
  name: "ComponentTest",  
  setup() {  
  
    const point = userPoint()  
  
    return {  
      point  
    }  
  }  
}
```


## toRef

用于创建一个ref对象，其value属性指向另一个对象中的某个属性

要将响应式对象中的某个属性单独提供个外部使用

toRefs功能和toRef相同，不同的是可以批量创建多个

```js
setup() {  
  
  const point = reactive({  
    x: 1,  
    y: 0  
  })  
  
  const z = toRef(point, 'x')  // 此时的 z 最终还是找到的 x（不是新生成的）  
  const {x, y} = toRefs(point)  // 只拆解第一层
  
  return {  
    x,  
    y,  
    z  
  }  
}
```

toRef最终还是指向的point中的数据，但是如果是使用`ref`那么是新生成一个响应式数据


