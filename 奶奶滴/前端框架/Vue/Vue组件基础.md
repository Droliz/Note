### vue 组件基础

#### 单页面应用程序

单页面应用程序：简称 SPA 指的是一个网站中只有唯一的一个 HTML 页面，所有功能在这一个页面中实现

仅仅在 web 页面初始化的时候加载相应的资源，一旦页面加载完成， SPA 不会因为用户的操作而**进行页面的重新加载或跳转**。而是利用 JS 动态的变换 HTML 的内容，从而实现页面与用户之间的交互

优点：
* 良好的交互体验
* 良好的前后端工作分离模式
* 减轻服务器压力  
缺点：
* 首屏加载慢（解决：路由懒加载、压缩代码、CDN加速、网络传输压缩）
* 不利于 SEO（解决：SSR 服务器端渲染）

官方提供两种快速创建工程化 SPA 项目的方式：
* 基于 vite 创建 SPA 项目
* 基于 vue-cli 创建 SPA 项目

| |vite|vue-cli|
|:--:|:--:|:--:|
|支持的vue版本|仅支持3.x|支持2.x、3.x|
|是否基于webpack|否|是|
|运行速度|快|较慢|
|功能完整度|小而巧（逐渐完善）|大而全|
|是否建议在企业级开发中使用|目前不建议|建议企业级开发|

#### vite 的基本使用

使用vite初始化一个 SPA 项目

```sh
# 初始化项目
npm init vite-app project_name
# 安装依赖包
cd project_name
npm install
# 启动项目
npm run dev
```

项目运行流程

在工程化的项目中，vue：通过 main.js 把 App.vue 渲染到 index.html 指定区域
* App.vue 用来编写带渲染的模板结构
* index.html 中需要预留一个el区域
* main.js 把 App.vue 渲染到 index.html 所预留的区域中

在.vue文件中，所有的模板都要写在`template`标签下

```vue
<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld msg="Hello Vue 3.0 + Vite" />
</template>
```

在main.js中进行渲染

```js
// createApp 创建 vue 的单页面应用程序实例
import { createApp } from 'vue'

// 导入待渲染的 App.vue 模板
import App from './App.vue'
import './index.css'

// 指定渲染的区域
createApp(App).mount('#app')
```

#### 组件化开发思想

根据封装的思想，把页面上可重复使用的部分封装为组，方便项目开发和维护

vue中规定组件的后缀为`.vue`

#### vue 组件的构成

组件的三个组成部分
* template -> 组件的模板结构
* script -> 组件的 js 脚本
* style -> 组件的样式

一个组件必须包含template，而js和样式都是可选的

**template节点**

template节点支持vue的指令

**script节点**

script节点中可以封装当前组件的js业务逻辑，通过 `export default {}` 导出。

可以在导出的对象中添加如下节点
* name：可以通过 name 节点定义当前的组件名称
* data：可以通过 data 节点传出渲染期间组件需要用到的数据
* methods：组件中的处理函数必须在methods节点中

```vue
<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      count: 0
    }
  },
  methods: {
	addcount() {
		this.count++;
	}
  }
}
</script>
```

**style节点**

style节点默认是css语法，lang属性规定使用的语法，可以使用css、less、scss等

```html
<style lang="css">
h1 {
  color: red;
}
</style>
```

组件分为
* 非单文件组件
	一个文件中包含n个组件（`.html`）
* 单文件组件
	一个文件只有一个组件（`.vue`）

##### 非单文件组件

```js
// 创建组件  
const person = Vue.extend({  
    template: `<h2>{{name}}--{{age}}</h2>`,  
    data() {   // 组件 data 必须是函数使用返回对象的形式（防止引用）  
        return {  
            name: "jack",  
            age: 1,  
        }  
    },  
})  

// 简写
const person = {
	// 配置项
}

// 注册
new Vue({  
    el: "#app",  
    components: {  
        person   // 局部注册
    }  
});

// 全局
Vue.component("person", person);
```

##### VueComponent构造函数

```js
const school = Vue.extend({  
    name: "school",  
    template: `  
        <h2>{{name}}</h2>    `,  
    data() {  
        return {  
            name: '武大'  
        }  
    }  
})

console.log(school)
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220903165315.png)

组件本质就是一个==构造函数==，由`Vue.extend`生成，当使用组件（标签）时，Vue解析时会创建组件的实例对象（`new VueComponent(options)`）==每次调用== `Vue.extend` ==都会返回一个全新的==。组件中的 this 指向为 `VueComponent` 实例对象

```js
const school = Vue.extend({  
    name: "school",  
    template: `  
    <div>        
	    <h2>{{name}}</h2>        
	    <button @click="show">ShowName</button>    
    </div>    
    `,  
    data() {  
        return {  
            name: '武大'  
        }  
    },  
    methods: {  
        show() {  
            console.log(this.name)  
            console.log(this)  
        }  
    }  
})
```


![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220903171026.png)

**Vue 和 VueComponent 的内置关系（非常重要）**

* `VueComponent.prototype.__proto__ == Vue.prototype`
* 关于函数原型、原型链参考 [JS](../JS.md) 中的构造函数

```js
const school = Vue.extend({  
    name: "school",  
})  
  
new Vue({  
    el: "#app",  
})  
  
console.log(school.prototype.__proto__ === Vue.prototype)  // true
```


![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220903192557.png)

Vue 强制让 `VueComponent` 原型对象指向 Vue 的原型对象，而不是指向 `Object` 的原型对象。这样可以让组件实例对象可以访问 Vue 原型上的属性和方法

```js
Vue.prototype.x = 1;  
  
const school = Vue.extend({  
    name: "school",  
    template: `<button @click="showX">showX</button>`,  
    methods: {  
        showX() {  
            console.log(this.x)  
        }  
    }  
})  
  
new Vue({  
    el: "#app",  
    components: {  
        school  
    }  
})
```

上述中，vc并没有属性 x，但是通过==原型链==，先在组件的原型对象找，然后会找到 vue原型上的 x ，如果vue原型上也没有x，才会去object上查找（`undefined`）


##### 单文件组件使用

单文件组件（`.vue`）浏览器是不识别的，需要处理为 js（借助webpack或脚手架）

在单文件中，Vue实例放在 `main.js` 中，只有 `App.vue` 根组件才可以在

#### 组件注册

* **组件的注册**

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220616211932.png)

组件之间可以相互的引用（先注册后使用）
* 全局注册组件：全局任何一个组件都可以使用

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220616212145.png)

```js
// main.js
// createApp 创建 vue 的单页面应用程序实例
import { createApp } from 'vue'
// 导入待渲染的 App.vue 模板
import App from './App.vue'
	  
import Swiper from './components/Swiper.vue'
		
const app = createApp(App)
// 注册全局组件（可以直接以标签的形式使用）标签一般使用短横线命名法或大驼峰命名法
// 不同的是，大驼峰命名法在使用的时候也可以使用短横线命名法的方式（my-swiper）
app.component('MySwiper', Swiper)   // （标签名，组件）
// 标签名可以使用   组件.name  形式直接用name注册

// 指定渲染的区域
app.mount('#app')
```

* 局部注册组件：在当前注册的范围内使用

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220616212225.png)
	
在需要的组件的script中注册（在components中添加，用标签的形式使用）

```vue
<!-- App.vue -->
<script>
import Test from "./components/Test.vue";

export default {
  name: "MyApp",
  data() {
	return {
	  username: "zs",
	  count: 0,
	};
  },
  components: {
	Test: Test,
  },
};
</script>
```

* 组件之间的样式冲突问题
	默认情况下在`.vue`组件中的样式会全局生效，导致很容易造成组件之间的样式冲突问题		
	例如：在父组件`App.vue`导入使用子组件`List.vue`时，更改父组件的样式，子组件的样式也会被更改
	解决：人为的为每个组件分配唯一的自定义属性，在编写样式时，通过属性选择器来控制样式的作用域

```vue
<template>
  <div data-v-001>
	<h1 data-v-001>这是 App.vue 组件</h1>
	<p data-v-001>App 中的 p 标签</p>
	<p data-v-001>App 中的 p 标签</p>
	<hr data-v-001 />
	<my-list data-v-001></my-list>
  </div>
</template>

<style lang='less'>
  p[data-v-001] {
	color: red;
  }
</style>
```

但是这样非常的影响开发的效率，所以在style节点下提供了scoped属性，从而防止组件之间的样式冲突问题（会自动给每个DOM元素分配唯一的自定义属性）

```vue
<style scoped lang='less'>
  p {
	  color: red;
  }
</style>
```

* deep样式穿透
	对于父组件如果设置了scoped属性，那么其样式是不会对子组件生效（每一个样式会自动加上属性选择器），如果想要某些样式对子组件生效，可以使用`deep()深度选择器`

```vue
<style scoped lang='less'>
.a :deep(.b) {
	// style
}
</style>
```

示例：

```vue
<style lang='less' scoped>
  p {
	color: red;
  }
  :deep(.title) {   // deep深度选择器
	color: blue;
  }
</style>
```

**组件的 props**

为了提高组件的复用性，在封装 vue 组件时需要遵守日下规则：
* 组件的 DOM 结构、Style 样式尽量复用
* 组件中要展示的数据，尽量有组件的使用者提供

props 是组件的自定义属性，组件的使用者可以通过 props 把数据传递到子组件内部，供子组件内部进行使用

```vue
<!-- 通过自定义 props，把文章的标题和作者，传递到 my-article 组件中 -->
<my-article title="键盘敲烂，月入过万" author="Droliz"></my-article>
```

将动态的数据项声明为 props 自定义属性。自定义属性可以在当前的组件的模板结构中被直接使用

```vue
<!-- Article.vue -->
<template>
    <div>
        <h3>标题 {{title}}</h3>
        <h3>作者 {{author}}</h3>
    </div>
</template>
  
<script>
export default {
    name: "MyArticle",
    // 外界传递指定的数据，到当前的组件的实例对象身上
    props: ['title', 'author']
    // 传参时，只用在标签中添加对应的属性和值即可，也可以是对象{属性名: 类型}
}
</script>
```

在使用自定义的属性时，可以使用`v-bind`指令传参，属性可以使用短横线命名法也可使用驼峰，与标签相同

**Class 与 style 绑定**

通过`v-bind`指令为元素动态绑定 class 属性的值和行内的 style 样式

class：可以使用三元表达式，动态的为元素绑定 class 的类名（简写 `:class="{类名: 布尔值}"`）

```vue
<!-- 通过点击按钮改变isItalic值，来确定class是否添加 -->
<h3 class=".thin" :class="isItalic ? 'italic' : ''"></h3>
<button @click="isItalic=!isItalic"></button>


data() {
	return {
		isItalic: true
	}
}

.thin {
	font-weight: 200
}

.italic {  <!-- 斜体 -->
	font-style: italic;
}
```

以数组的语法绑定（绑定多个，但是会导致语法臃肿）

```vue
<h3 class=".thin" :class="[isItalic ? 'italic' : '', isDelete ? 'delete' : '']"></h3>
```

以对象的语法绑定

```vue
<h3 class=".thin" :class="classObj"></h3>
<button @click="classObj.italic = !classObj.italic"></button>
<button @click="classObj.delete = !classObj.delete"></button>

data() {
	return {
		classObj: {  <!-- 类名: 布尔值 true表示添加此class -->
			italic: true,
			delete: false
		}
	}
}
```

以对象语法绑定内敛的style样式

```vue
<!-- 如果有短横线，要么改为驼峰，要么加引号代表字符串 -->
<div :style="{color: sctive, fontSize: fsize + 'px', 'background-color': bgcolor}">

data() {
	return {
		active: 'red',
		fsize: 30,
		bgcolor: 'pink'	
	}
}
```


#### props 验证

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220617132221.png)

在封装组件时，对外界传递过来的 props 数据进行合法性校验，从而防止数据不合法问题

可以使用对象类型的 props

```vue
<School name="jack" :age="18"></School>

<script>
export default {
	props: {
		name: String,
		age: Number,
	}
}
</script>
```

|数据类型|说明|
|:--:|:--:|
|String|字符串类型|
|Number|数字类型|
|Boolean|布尔值类型|
|Array|数组类型|
|Object|对象类型|
|Date|日期类型|
|Function|函数类型|
|Symbol|符号类型|

除了直接指定校验类型之外，还可以指定多个可能的类型、必填项校验、属性默认值、自定义验证函数

```vue
<script>
export default {
	props: {
		// 多个可能值
		propA: [String, Number],
		// 必填项校验
		propB: { type: String, required: true },
		// 属性默认值
		propC: { type: Number, default: 100 },
		// 自定义验证函数
		propD: {
			// 通过validator函数，对propD属性的值进行校验
			validator(value) {
				// 属性值必须匹配下载字符串中的一个
				return ['success', 'warning', 'danger'].indexOf(value) !== -1
			}
		} 
	}
}
</script>
```


#### mixins混合

如果多个组件都用到了同一个配置项（钩子函数、回调等等），那么可以将此回调函数写到公共的`js`文件（`mixin.js`）中，然后再需要使用的组件中导入，在 `mixins` 配置项中配置

混合可以理解为组件复用配置，混合中配置的优先级是低于组件自己配置的优先级的。==但是对于生命周期钩子都会保留，且此时会先执行混合中定义的生命周期钩子，再执行组件中定义的生命周期钩子==

```js
// mixin.js
const meth = {  
    methods: {  
        showName() {  
            alert(this.name)  
        }  
    }  
}  
  
export {  
    meth,  
}


// School.vue
<template>  
  <div>  
    <h1 @click="showName">{{ name }}</h1>  
  </div>  
</template>  
  
<script>  
import {meth} from "@/mixin";  
  
export default {  
  name: "School",  
  data() {  
    return {  
      name: '111'  
    }  
  },  
  mixins: [meth]   // 混合配置项
}  
</script>  
```

如果向配置全局混合，在`main.js`中引入混合调用`Vue.mixin`即可

```js
// main.js
import {meth} from "@/mixin";
Vue.mixin(meth)
```

#### 计算属性

通过已有的属性计算得来

计算属性本质上就是一个 function 函数，它可以实时监听 data 中数据的变化，并 return 一个计算后的新值，供组件渲染 DOM 时使用

```js
new Vue({  
    el: '#app',  
    data: {  
        name1: "",  
        name2: "",  
    },  
    computed: {  
       fullName: {    // 在对象中配置
            get() {    // get函数，在初次加载 fullName 时调用，以及依赖的数据改变时调用
                return this.name1 + " - " + this.name2;  
            },  
            set(value) {
	            
            }  
        },  
    }  
});

// fullName 可以简写为函数形式
fullName() {
	return this.name1 + " - " + this.name2;
}
```

* get：当需要读取计算属性时，自动调用get，返回fullName 的值，在缓存中保存，当依赖值改变则再次调用并保存到缓存
* set：当计算属性被修改时调用

计算属性的简写只在不需要修改计算属性（setter）时才会使用

示例：
```vue
<template>
  <div>
	<!-- v-model 双向绑定 -->
    <input type="text" v-model.number="count" />
    <p>{{ count }} * 2 的值为 {{ plus }} </p>   <!-- count为原始的，plus为计算过后的 -->
  </div>
</template>
  
<script>
export default {
  name: "MyApp",
  data() {
    return {
      count: 1
    }
  },
  computed: {
    plus() {
	  // 必须要有返回值、当作普通的属性使用，不当作方法或函数	
	  return this.count * 2;
    }
  }  
  // 如果通过methods实现，那么表达式中必须是函数的调用，不能只写函数名
};
</script>
```

相对于方法，计算属性会缓存计算的结果，只有依赖项发生变化时，才会重新进行运算。由此，计算属性的性能好（购物车的商品总个数和总计）

```vue
<template>
  <div>
	<!-- 对于同一个操作，方法会调用多次，而计算属性只调用一次 -->
    <input type="text" v-model.number="count" />
    <p>{{ count }} * 2 的值为 {{ plus }}</p>
    <p>{{ count }} * 2 的值为 {{ plus }}</p>
    <p>{{ count }} * 2 的值为 {{ func() }}</p>
    <p>{{ count }} * 2 的值为 {{ func() }}</p>
  </div>
</template>

<script>
export default {
  name: "MyApp",
  data() {
    return {
      count: 1,
    };
  },
  computed: {
    plus() {
      console.log('计算属性');
      return this.count * 2;
    },
  },
  methods: {
    func() {
      console.log('方法');
      return this.count * 2;
    },
  }
};
</script>

<!-- output -->
<!--
计算属性
2 方法
-->
```

在计算属性中使用过滤器

```js
let a = 0
this.fruitlist
  .filter(x => x.state)   // 过滤器
  .forEash(x => {
    a += x.price * x.count
  })
```

示例
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
    <script src="../lib/vue-2.6.14.js"></script>  
</head>  
<body>  
<div id="app">  
    <span>天气: {{weather}}</span>  
    <button @click="change">Change</button>  
</div>  
<script>  
    Vue.config.productionTip = false;  
  
    new Vue({  
        el: "#app",  
        data: {  
            temp: "cool",  
        },  
        methods: {  
            change() {  
                this.weather = this.weather === "hot" ? "cool" : "hot";  
            }  
        },  
        computed: {  
            weather: {  
                get() {  
                    return this.temp;  
                },  
                set(value) {  
                    this.temp = value;  
                }  
            }  
        },  
        // 直接使用 侦听器实现  
        watch: {  
            weather: {  
	            // 当监听的属性发生改变时调用
                handler(newVal, oldVal) {  
                    console.log(newVal, oldVal);  
                }  
            }  
        }  
    });  
  
</script>  
</body>  
</html>
```


#### plugins插件

插件本质是一个对象，用于增强 Vue 功能

src下创建 `plugins.js`

```js
// plugins.js
import {meth} from "@/meth.js"

export default {  
	// 第一个参数是 Vue 构造函数，后面为传入的参数
    install(Vue, a, b) {    
        console.log(Vue) // Vue function .....
        console.log(a)   // 1
        console.log(b)   // 2
		// 插件中可以定义全局过滤器、全局指令、给Vue原型添加方法和属性
		// 包括混合也可以定义在插件中
		Vue.mixin(meth)
    }  
}

// main.js
import plugins from "@/plugins";
Vue.use(plugins, 1, 2)  // 使用插件，后面是参数
```


#### 自定义事件

在自定义组件时，为了使用者可以监听到组件内的状态变化，需要用到组件的自定义事件（==子传父==），在之前实现子传父需要父提前准备一个函数，然后通过 props 传给子，子再调用此函数达到传递参数的目的

自定义事件，给那个组件绑定的自定义事件就在组件中触发和解绑

步骤：
* 在子组件的`emits`配置项声明自定义事件
* 在子组件合适的时机触发自定义事件`this.$emit.事件名称`
* 在父组件中配置回调函数以及绑定自定义事件`v-on`或`ref`

**声明自定义事件**

在子组件的emits节点下声明自定义事件

```vue
<!-- Counter.vue -->
<template>
  <div>
    <input type="text" v-model.number="count" />
    <button @click="onBtnClick">+1</button>
    <button @click="unbind">解绑自定义事件</button>
  </div>
</template>

<script>
export default {
	// 自定义事件，必须先在 emits 中声明
	emits: ['count-change'],
}
</script>
```

**触发与解绑自定义事件**

对于上述在 emits 配置项下声明的自定义事件，可以通过`this.$emit('自定义事件名称', 参数)`的方法触发

解绑一般在[生命周期钩子](#生命周期)中

```vue
<!-- Counter.vue -->
<script>
export default {
	methods: {
		onBthClick() {
			this.count++;
			// 通过第二个参数传参
			this.$emit('count-change', this.count);  // 触发自定义事件  'count-change'
		}
		unbind() {
			this.$off('count-change')   // 解绑一个自定义事件
			// this.$off(['event1', 'event2'])  // 解绑多个自定义事件，或不传参(解绑所有)
		}
	}
}
</script>
```

**监听自定义事件**

在使用组件时，可以通过 `v-on` 的形式绑定自定义事件。

对于上述的 count 组件，在父组件 `App.vue` 中监听事件

```vue
<!-- App.vue -->
<template>
	<my-counter @count-changed="getCount"></my-counter>
</template>

<script>
export default {
 methods: {
	// 通过形参直接接收
    getCount(count) {
      console.log("触发自定义事件：count-changed，count=", count);
    },
  },
}
</script>
```

通过自定义事件传递数据的方式，不需要使用 `props`，但是父组件都需要回调函数接收子组件传来的参数。但是相较于`props`，自定义事件不需要将回调传给子组件

除此之外可以通过 [ref](#ref引用) 获取组件对象来绑定事件（灵活），需要注意绑定事件一般在 `mounted()` 中（详情见 [生命周期](#生命周期)）

```vue
<!-- App.vue -->
<template>
	<my-counter ref="Count"></my-counter>
</template>

<script>
export default {
 methods: {
	// 通过形参直接接收
    getCount(count) {
      console.log("触发自定义事件：count-changed，count=", count);
    },
  },
  mounted() {
	  // 改为 $once 表示一次
	  this.$refs.Count.$on("count-changed", this.getCount)  
  }
}
```

**注意**

通过 `ref` 的形式绑定自定义事件时，回调函数如果不使用箭头函数或者不在`methods`中提前定义，那么此回调函数中的this指向的是触发事件的对象

如果通过`v-on`给组件绑定原生事件，组件会当作自定义事件，需使用`native`修饰符才能认为是原生事件（`@click.native="..."`）


#### 全局事件总线

可以实现任意组件之间的通信

通过另外的一个组件 `X` 作为中间组件，当有组件`A`需要获取数据，那么在`A`中对`X`绑定自定义事件并在`A`中编写事件的回调函数，如果需要从组件 `B` 想组件 `A` 传递，那么在`B`中触发`X`上的由`A`绑定的自定义事件

中间组件 `X` 
* 所有的组件都可以访问
* 至少能调用 `$on、$off、$once、$emit`

所有的组件都可以看到Vue的原型上的属性和方法（[内置关系](#VueComponent构造函数)）

Vue 的原型对象上的属性和方法，所有的组件实例对象都可以访问，而且 Vue  的原型对象也有这些方法

直接在 `mian.js` 中创建组件实例对象，并通过`prototype`方法将实例对象绑定到Vue原型对象上

```js
// main.js

// 创建组件实例对象
const Demo = Vue.extend({});
const d = new Demo();    

// 绑定
Vue.prototype.x = d;
```

除了上述的 vc 可以满足要求之外，vm 也是可以满足要求的

```js
// main.js

new Vue({  
  render: h => h(App),  
  beforeCreate() {  
    Vue.prototype.$bus = this;     // 安装全局事件总线
  }  
}).$mount('#app')
```

两个组件之间的传参

School.vue
```vue
<template>  
  <div>  
    <hr>  
    <h2>School  Name :  {{name}}</h2>  
    <hr>  
  </div>  
</template>  
  
<script>  
export default {  
  name: "School",  
  data() {  
    return {  
      name: "School",  
    }  
  },  
  methods: {  
    getName(val) {  
      console.log(val);  
      this.name = val;  
    }  
  },  
  mounted() {  
    this.$bus.$on('getName', this.getName);  
  },  
  beforeDestroy() {  
    this.$bus.$off("getName");  
  }  
}  
</script>  
  
<style scoped>  
  
</style>
```

Student.vue
```vue
<template>  
  <div>  
    <hr>  
    <h2>School  Name  :  {{name}}</h2>  
    <button @click="func">给名字</button>  
    <hr>  
  </div>  
</template>  
  
<script>  
export default {  
  name: "Student",  
  data() {  
    return {  
      name: "jack",  
    }  
  },  
  methods: {  
    func() {  
      this.$bus.$emit("getName", this.name);  
    }  
  }  
}  
</script>  
  
<style scoped>  
  
</style>
```

由于所有的事件都在 X 上，所以事件是不能重复的，而且为了减轻压力，需要在销毁组件之前对事件进行解绑

接收数据
```js
methods： {
	callback(val.....) {
		// code
	}
}

mounted() {
	this.$bus.$on('xxx', callback)
},
beforeDestroy() {
	this.$bus.$off('xxx')
}
```

传递数据
```js
this.$bus.$emit('xxx', data....)
```

#### 消息订阅与发布

借助第三方库实现例如 `pubsub.js` 实现

```js
npm i -S pubsub-js
npm i -S pubsub
```

由数据接收者订阅，由数据的提供者发布

接收数据
```js
import pubsub from "pubsub-js"  
  
export default {  
  name: "School",  
  data() {  
    return {  
      name: "School",  
    }  
  },  
  methods: {  
    getName(msgName, val) {   // 第一个参数是消息名，第二个才是传过来的参数  
      console.log(msgName, val);  
      this.name = val;  
    }  
  },  
  mounted() {  
    // 每次订阅都会生成一个 Id ， 销毁需要通过 Id 
    this.pubId = pubsub.subscribe('getName', this.getName);  // 消息名   回调函数
    console.log(this);  
  },  
  beforeDestroy() {  
    pubsub.unsubscribe(this.pubId);    // 销毁
  }  
}
```

```js
import pubsub from "pubsub-js"  
  
export default {  
  name: "Student",  
  data() {  
    return {  
      name: "jack",  
    }  
  },  
  methods: {  
    func() {  
      pubsub.publish('getName', this.name);    // 发布消息   消息名   参数
    }  
  }  
}
```


#### 组件上的v-model

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220618062801.png)

当需要维护组件内外数据的同步时，也可以在组件上使用 `v-model` 指令

**父传子**

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220618071340.png)

父组件通过 `v-bind:` 属性绑定的形式，把数据传递给父组件
子组件中通过 props 接收父组件传递过来的数据

**子传父**

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220618071414.png)

在 `v-bind:` 指令之前添加 `v-model` 指令，使用自定义事件的形式将数据传递给父组件


**组件双向绑定数据的案例**

```vue
<!-- App.vue -->
<template>
  <div>
    <h1>这是 App.vue 根组件</h1>
    <p>父组件中的 count = {{ count }}</p>
    <button @click="count++">+1</button>
    <hr />
    <my-counter v-model:number="count"></my-counter>
  </div>
</template>
 
<script>
import MyCounter from "./components/Counter.vue";
export default {
  name: "MyApp",
  data() {
    return {
      count: 0,
    };
  },
  components: {
    MyCounter,
  },
  methods: {
    // 接收子传过来的参数
    countChanged(count) {
      this.count = count;
    },
  },
};
</script>
  
<!-- Counter.vue -->
<template>
  <div>
    <h2>这是 Counter 组件</h2>
    <button @click="changeCount">+1</button>
    <p>子组件中的 Count = {{ number }}</p>
  </div>
</template>
  
<script>
export default {
  name: "MyCounter",
  props: ["number"],
  emits: ["update:number"], // 更新的数据
  methods: {
    changeCount() {
	  // 注意如果直接写进去，不要用 this.number++
      this.$emit("update:number", this.number + 1);
    },
  },
};
</script>
```


##### 任务列表案例

**最终效果**

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220618195632.png)

**项目初始化**

使用 vite 初始化一个 SPA 项目

```sh
npm init vite-app project_list
```

安装依赖

```sh
npm install
```

安装 less 依赖

```sh
npm i less -D
```

项目的 src 目录结构

```txt
src
│  App.vue
│  index.css
│  main.js
│
│
├─assets    // 静态资源目录
│
│
└─components    // 组件目录
    ├─todo-button
    │      TodoButton.vue   // 按钮组件
    │
    ├─todo-input
    │      TodoInput.vue    // 输入框组件
    │
    └─todo-list
            TodoList.vue    // 列表组件

```

重置 `css` 全局样式

```css
:root {
  font-size: 12px;
}

body {
  padding: 8px;
}
```

重置 `App.vue` 组件

```vue
 <template>
  <div>
    <h1> App 根组件</h1>
  </div>
</template>
  
<script>
export default {
  name: "MyApp",
  data() {
    return {
      // 任务列表 格式 {id: 1, task: "吃饭", done: "false"}
      todoList: [],
      // 下一个id
      nextId: 1
    };
  }
}
</script>
  
<style lang="less" scoped>
  
</style>
```

**封装 todo-list 组件**

1、创建并注册 TodoList 组件

在 `src/components/todo-list/` 目录下新建 `TodoList.vue` 组件

```vue
<!-- TodoList.vue -->
<template>
    <div>
        <h1> TodoList 组件</h1>
    </div>
</template>

<script>
export default {
    name: "TodoList",
}
</script>

<style lang="less" scoped> 

</style>
```

在 `App.vue` 组件中注册 `TodoList.vue` 组件

```vue
<!-- App.vue -->
<script>
import TodoList from './components/todo-list/TodoList.vue'
export default {
  components: {
    TodoList
  },
}
</script>
```

2、基于 bootstrap 渲染组件

使用bootstrap的[列表](https://v4.bootcss.com/docs/components/list-group/)和[复选框](https://v4.bootcss.com/docs/components/forms/)具体参考 bootstrap v4 官方文档

```vue
<!-- TodoList.vue -->
<template>
  <ul class="list-group">
    <!-- 列表组 -->
    <li class="list-group-item d-flex justify-content-between align-items-center">
    <!-- 复选框 -->
      <div class="custom-control custom-checkbox">
        <input type="checkbox" class="custom-control-input" id="customCheck1" />
        <label class="custom-control-label" for="customCheck1">Check this custom checkbox</label>
      </div>
      <span class="badge badge-success badge-pill">完成</span>
      <span class="badge badge-warning badge-pill">未完成</span>
    </li>
  </ul>
</template>
```

3、声明 `props` 属性

```vue
<!-- TodoList.vue -->
<script>
export default {
	props: {
        list: {
            type: Array,
            required: true,
            default: []
        }
    }
}
</script>
```

在 `App.vue` 组件中传参

```vue
<!-- App.vue -->
<template>
<todo-list :list="todoList"></todo-list>
</template>
```

4、渲染 DOM 结构

通过 `v-for` 渲染 DOM 结构

```vue
<!-- TodoList.vue -->
<template>
  <ul class="list-group">
    <!-- 列表组 -->
    <li class="list-group-item d-flex justify-content-between align-items-center" v-for="item in list" :key="item.id">
      <!-- 复选框 -->
      <div class="custom-control custom-checkbox">
        <input type="checkbox" class="custom-control-input" :id="item.id" />
        <label class="custom-control-label" :for="item.id">{{ item.task }}</label>
      </div>
  </ul>
</template>
```

通过 `v-if` 和 `v-else` 选择渲染

```html
<!-- TodoList.vue -->
<span class="badge badge-success badge-pill" v-if="item.done">完成</span>
<span class="badge badge-warning badge-pill" v-else>未完成</span>
```

通过 `v-model` 双向绑定

```vue
<!-- TodoList.vue -->
<input type="checkbox" class="custom-control-input" :id="item.id" v-model="item.done"/>
```


通过 `v-bind` 动态绑定样式

```vue
<!-- TodoList.vue -->
<label class="custom-control-label" :class="{delete: item.done}" :for="item.id">
```

样式

```vue
<!-- TodoList.vue -->
<style>
.list-group {
    width: 400px;
}
 

/* 删除效果 */
.delete {
  text-decoration: line-through;
  color: gray;
  font-style: italic;
}
</style>
```

**封装 todo-input 组件**

1、创建并注册 TodoInput 组件

在 `src/components/todo-input/` 目录下新建 `TodoInput.vue` 组件

```vue
<!-- TodoInput.vue -->
<template>
    <div>
        <h1> TodoInput 组件</h1>
    </div>
</template>

<script>
export default {
    name: "TodoInput",
}
</script>

<style lang="less" scoped> 

</style>
```

在 `App.vue` 组件中注册 `TodoInput.vue` 组件

```vue
<!-- App.vue -->
<script>
import TodoInput from './components/todo-input/TodoInput.vue'
export default {
  components: {
    TodoInput,
  },
}
</script>
```

2、基于 bootstrap 渲染组件

```vue
<!-- TodoInput.vue -->
<template>
<!-- from 表单 -->
<form class="form-inline">
  <div class="input-group mb-2 mr-sm-2">
	<!-- 输入框前缀 -->
	<div class="input-group-prepend">
	  <div class="input-group-text">任务</div>
	</div>
	<!-- 输入框 -->
	<input
	  type="text"
	  class="form-control"
	  placeholder="请填写任务"
	  style="width: 356px"
	/>
  </div>
  <!-- 提交按钮 -->
  <button type="submit" class="btn btn-primary mb-2">添加任务</button>
</form>
</template>
```

3、通过自定义事件向外传递数据

添加的任务需要传入到 `App.vue` 中的todoList数组中去，使用自定义事件传递

声明数据

```vue
<!-- TodoInput.vue.vue -->
data() {
	return {
	  task: '',
	};
},
```

`v-model` 双向绑定数据

```vue
<!-- TodoInput.vue -->

<!-- 输入框进行v-model双向数据绑定 -->
<input
  type="text"
  class="form-control"
  placeholder="请填写任务"
  style="width: 356px"
  v-model.trim="task"    
/>
```

阻止表单默认提交，声明自定义事件，并指定事件处理函数

```vue
<!-- TodoInput.vue -->

<!-- 阻止 from 表单默认提交，并指定处理函数 -->
<form class="form-inline" @submit.prevent="onFormSubmit">

<script>
export default {
    emits: ['add'],
	methods: {
	    // 定义处理函数
	    onFormSubmit() {
	        // 如果输入框为空，则不添加任务
	        if (!this.task) {
	          return;
	        }
	        // 添加任务
	        this.$emit('add', this.task);
	        // 清空输入框
	        this.task = '';
	    }
	}
}
</script>
```

4、在 `App.vue` 组件中监听 `add` 自定义事件

```vue
<!-- App.vue -->
<template>
    <todo-input @add="addTask"></todo-input>
</template>

<script>
export default {
	methods: {
    addTask(task) {
      // 添加任务
      this.todoList.push({
        id: this.nextId,
        task: task,
        done: false
      });
      // 更新下一个任务的 id
      this.nextId++;
    }
  }
}
</script>
```


**TodoButton 组件**

1、创建并注册 TodoInput 组件

在 `src/components/todo-button/` 目录下新建 `TodoButton.vue` 组件

```vue
<!-- TodoButton.vue.vue -->
<template>
    <div>
        <h1> TodoButton 组件</h1>
    </div>
</template>

<script>
export default {
    name: "TodoButton",
}
</script>

<style lang="less" scoped> 

</style>
```

在 `App.vue` 组件中注册 `TodoInput.vue` 组件

```vue
<!-- App.vue -->
<script>
import TodoButton from './components/todo-button/TodoButton.vue'
export default {
  components: {
    TodoButton,
  },
}
</script>
```

2、基于 bootstrap 渲染组件

```vue
<!-- TodoButton.vue.vue -->
<template>
<div class="button-container mt-3">
  <div class="btn-group" role="group" aria-label="Basic example">
    <button type="button" class="btn btn-secondary">全部</button>
    <button type="button" class="btn btn-secondary">已完成</button>
    <button type="button" class="btn btn-secondary">未完成</button>
  </div>
</div>
</template>

<style lang="less" scoped>
.button-container {
  width: 400px;
  text-align: center;
}
</style>
```

3、通过 `props` 指定默认激活的按钮

在props中声明，默认值为 0，0：全部、1：已完成、2：未完成

```vue
<!-- TodoButton.vue.vue -->
<script>
export default {
  props: {
    // 激活项的索引
    active: {
        type: Number,
        required: true,
        default: 0,
    }
  }
}
</script>

<!-- 在 App.vue 中添加 activeBtnIndex -->
<script>
export default {
	data() {
		return {
			activeBtnIndex: 0
		}
	},
	
}
</script>

<!-- 通过属性绑定的形式传递给 TodoButton 组件 -->
<todo-button :active="activeBtnIndex"></todo-button>
```

给按钮动态绑定类名

```html
<!-- TodoButton.vue.vue -->

<button type="button" class="btn" :class="active === 0 ? 'btn-primary' : 'btn-secondary'">全部</button>
<button type="button" class="btn" :class="active === 1 ? 'btn-primary' : 'btn-secondary'">已完成</button>
<button type="button" class="btn" :class="active === 2 ? 'btn-primary' : 'btn-secondary'">未完成</button>
```

4、通过 `v-model` 更新激项的索引

>父  --->  子   通过 props 传递激活项的索引
>子  --->  父   更新父组件中的 activeBtnIndex

绑定按钮点击事件

```vue
<!-- TodoButton.vue.vue -->
<button type="button" class="btn" :class="active === 0 ? 'btn-primary' : 'btn-secondary'" @click="btnClick(0)">全部</button>
<button type="button" class="btn" :class="active === 1 ? 'btn-primary' : 'btn-secondary'" @click="btnClick(1)">已完成</button>
<button type="button" class="btn" :class="active === 2 ? 'btn-primary' : 'btn-secondary'" @click="btnClick(2)">未完成</button>
```

声明自定义事件，用来更新父组件通过 `v-model` 指令传递过来的 `props` 数据

```vue
<!-- TodoButton.vue.vue -->
<script>
export default {
  // 声明自定义事件
  emits: ["update:active"],
  methods: {
    // 点击处理函数
    btnClick(index) {
     if (this.active === index) {
        return;
      }
      // 触发自定义事件
      this.$emit("update:active", index);
    },
  },
};
</script>
```

通过计算属性动态更改列表

根据当前的 `App.vue` 中的 `activeBtnIndex` 值，选择性的向 `TodoList.vue` 传入需要的列表数据

在 `App.vue` 中使用计算属性

```vue
<script>
	computed: {
		// 根据索引值，动态计算需要渲染的列表数据
		taskList() {
		  switch (this.activeBtnIndex) {
			case 0:
			  return this.todoList;
			case 1:
			  return this.todoList.filter((item) => item.done);
			case 2:
			  return this.todoList.filter((item) => !item.done);
			default:
			  return this.todoList;
		  }
		}
	}
</script>

<!-- 更改传入的参数 -->
<todo-list :list="taskList" class="mt-2"></todo-list>
```



#### watch 侦听器

watch 侦听器允许开发者监视数据的变化，从而针对数据的变化做特定的操作（监视用户名的变化并发起请求，判断用户名是否可用） 相比计算属性，侦听器中可以进行异步操作（定时器等）

所有不被vue管理的函数（定时器回调、ajax回调）都写成箭头函数，this指向才是vue实例对象

**watch 侦听器基本语法**

在 watch 节点下，定义自己的侦听器

```vue
// MyWatch.vue

<template>
  <div>
    <h1> App.vue </h1>
    <my-watch></my-watch>
  </div>
</template>
 
<script>
import MyWatch from './components/MyWatch.vue';
  
export default {
  name: 'MyApp',
  components: {
    MyWatch,
  },
};
</script>
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220618205838.png)
* immediate选项
	* 在默认情况下，初次渲染完成是不会调用侦听器的，如果需要初次就开始调用需要使用immediate属性

```js
watch: {
	username: {
		// 会自动调用handler函数(固定写法)
		async handler(newVal, oldVal) {
			const res = await axios.get("url" + newVal)
			console.log(res)
		},
		immediate: true,  
	}
}
```

* deep选项
	* 如果 watch 侦听的是对象，如果对象的属性值变化，watch是无法侦听到的

```js
data() {
	return {
		info: {
			username: 'zs'
		}
	}	
}

watch: {
	// 将 info 改为 'info.username' 代表单独监听对象 info 中的 username 属性
	info: {
		async handler(newVal) {
			const { data: res } = await axios.get("url" + newVal)
			console.log(res)
		},
		// 深度侦听（当单独监听一个属性时，可以不用 deep ）
		deep: true
	}
}
```

如果对象中有多个属性，更改其他不必要的属性时，也会触发侦听器，所以需要单独特定的侦听对象中的某一元素

计算属性侧重于多个数据的监听并返回一个新值，侦听器侧重单个数据变化，最终执行特定的业务逻辑，不需要任何返回值

在不需要以上选项，只需要handler时，可以类似[计算属性](#计算属性)简写

```js
watch: {
	info(newVal, ovlnewValal) {
		const { data: res } = await axios.get("url" + newVal)
		console.log(res)
	}
}
```

查看github账户信息

在输入框输入项查询的用户名，在控制台就会打印出该用户的对应信息

```vue
<!-- MyWatch.vue -->
<template>
  <div>
    <h1>watch 侦听器</h1>
    <p>
      <input type="text" v-model.trim="username" />
    </p>
  </div>
</template>
  
<script>
import axios from "axios";
  
export default {
  name: "MyWatch",
  data() {
    return { username: "" };
  },
  watch: {
    username: {
      // 自动调用handler函数
      async handler(newVal, oldVal) {
        let res = await axios.get(
          `https://api.github.com/users/${newVal}`
        );
        console.log(res);
      },
    },
  },
};
</script>


<!-- App.vue -->
<template>
  <div>
    <h1> App.vue </h1>
    <my-watch></my-watch>
  </div>
</template>
  
<script>
import MyWatch from './components/MyWatch.vue';
  
export default {
  name: 'MyApp',
  components: {
    MyWatch,
  },
};
</script>
```

#### 组件的生命周期

**组件的运行过程**

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220619162439.png)

**监听组件的不同时刻**

vue 框架为组件内置了不同时刻的生命周期函数，会伴随组件的运行自动调用
* 组件在内存中创建完毕之后，会调用 created 函数
* 被成功渲染到页面上，会调用 mounted 函数
* 被销毁之后，会调用 unmounted 函数

```vue
<!-- App.vue -->
<button @click="flag = !flag">隐藏组件</button>
<my-test v-if="flag"></my-test>

<script>
export default {
  data() {
    return {
      flag: true,
    };
  },
};
</script>

<!-- Test.vue -->
<script>
export default {
  name: 'Test',
  created() {
    console.log('created');
  },
  mounted() {
    console.log('mounted');
  },
  unmounted() {
    console.log('unmounted');
  },
};
</script>

<!--
第一次进入页面时会显示
created
mounted
当点击按钮会显示
unmounted
再次点击按钮会显示
created
mounted
-->
```

**监听组件的更新**

当组件的 data 数据更新之后，vue 会自动重新渲染组件的 DOM ，从而保证数据的一致性。当重新渲染完毕会自动调用 `updated` 生命周期函数

**组件中的主要的生命周期函数**

|生命周期函数|执行时机|阶段|单周期执行次数|应用场景|
|:--:|:--:|:--:|:--:|:--:|
|`created`|组件在内存中创建完毕|创建阶段|1|发起 ajax 请求|
|`mounted`|组件初次渲染完毕|创建阶段|1|操作 DOM 元素|
|`updated`|组件被重新渲染完毕|运行阶段|0或多|-|
|`unmounted`|组件被销毁后（内存与页面）|销毁阶段|1|-|

除此之外还有一些其他的生命周期函数。参考[vue官方文档](https://v3.cn.vuejs.org/api/options-lifecycle-hooks.html#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%92%A9%E5%AD%90)

|生命周期函数|执行时机|阶段|单周期执行次数|应用场景|
|:--:|:--:|:--:|:--:|:--:|
|`beforeCreate`|组件在内存中创建之前|创建阶段|1|-|
|`beforeMount`|组件渲染到页面之前|创建阶段|1|-|
|`beforeUpdate`|组件被重新渲染之前|运行阶段|0或多|-|
|`beforeUnmount`|组件被销毁之前|销毁阶段|1|-|

#### 组件之间的数据共享

**组件之间的关系**
* 父子关系
* 兄弟关系
* 后代关系

**父子之间的数据共享**
* 父 ---> 子
	父组件通过 `v-bind` 属性绑定向子组件共享，同时子组件需要使用 `props` 接收数据
* 子 ---> 父 
	子组件通过自定义事件向父组件共享，父组件需要监听自定义事件并通过形参接收数据
* 父 <--> 子 双向数据同步
	通过 `v-model` 维护组件内外数据的双向同步，在父组件传递数据的时候使用 `v-model` 指令，同时声明自定义事件（固定写法 ： update:需要修改的属性名）

**兄弟之间的数据共享**

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220619200947.png)

使用 EventBus 实现（第三方包 mitt 中的 eventBus 对象）

数据的接收方通过 `.on` 方法注册自定义事件，数据的发送方通过 `.emit` 方法触发自定义事件，并发送数据

```js
<!-- 数据接收方 Right.vue -->
export default {
  name: "Right",
  data() {
    return {
      num: 0,
    };
  },
  created() {
    // 注册自定义事件，通过事件处理函数的形参接收数据
    bus.on("countChange", (count) => {
      this.num = count;
    });
  },
};


<!-- 数据发送方 Left.vue -->
import bus from "./eventBus.js";
export default {
  name: "Left",
  data() {
    return {
      count: 0,
    };
  },
  methods: {
	// 点击事件的处理函数
    add() {
      this.count++;
      // 调用 bus.emit 触发事件
      bus.emit("countChange", this.count);
    },
  },
};


<!-- 公共的 EventBus 模块 eventBus.js -->
import mitt from 'mitt';
const bus = mitt();
export default bus
```

**后代关系组件之间的数据共享**

父节点的组件向子孙组件共享数据，可以使用 `provide` 和 `inject` 实现

父节点的组件通过 `provide` 方法，对子孙组件共享数据

```vue
<script>
export default {
  data() {
    return {
      color: 'red',   // 定义共享的数据
    };
  },
  provide() {   // return 出提供的数据
    return {
      color: this.color,  
    }
  },
}
</script>
```

子孙节点通过 `inject` 方法，接收父组件共享的数据

```vue
<script>
export default {
	// 子孙组件使用 inject 接收共享的数据
	inject: ['color']
}
</script>
```

**父节点对外共享响应式数据**

在上述共享中，如果父级变化，子孙级不会响应式的变化，此时需要结合computed函数向下共享

此时子孙节点如果要使用该数据，需要采用 `数据名.value` 的形式

```js
<!-- 父节点 -->
import { computed } from 'vue';

export default {
	data() {
		return { color: 'red' }
	},
	provide() {
		return {
			// 使用 computed 函数，将共享的数据'包装为'响应式的数据
			color: computed(() => this.color);
		}
	}
}
```

**vuex**

vuex 是终极的组件之间的数据共享方案（多个组件依赖于同一个状态），可以让组件之间的数据共享变得 清晰、高效、且易于维护

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220619223350.png)


使用 vuex 的项目，会将所有的数据共享存在 STORE 中，使得关系清晰（适合大范围、频繁的使用数据共享的项目）

**总结**

* 父子关系
	* 父 ---> 子  属性绑定
	* 子 ---> 父  事件绑定
	* 父 <-> 子  组件上的 v-model
* 兄弟关系
	* EventBus：全局事件总线
* 后代关系
	* provide & inject
* 全局数据共享
	* vuex

#### vue3.x中全局配置 axios

在实际项目开发中，需要使用 axios 发起数据请求
* 每个组件都导入 axios 会导致代码臃肿
* 每次请求都需要完整的请求路径，不利于维护

**在 main.js 配置**

在 `main.js` 入口文件中，通过 `app.config.globalProperties` 全局挂载 axios

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220619232808.png)

```js
// main.js
// createApp 创建 vue 的单页面应用程序实例
import { createApp } from 'vue'

// 导入待渲染的 App.vue 模板
import App from './App.vue'
import axios from 'axios'

const app = createApp(App)

// 配置请求根路径
axios.defaults.baseURL = 'http://api.droliz.cn/api/'
// 将 axios 挂载为 app 的全局自定义属性 后面的 http 名字自定义
app.config.globalProperties.$http = axios
```


#### vuex

专门在 Vue 中实现集中式状态（数据）管理的一个 Vue 插件，也是一种组件间的通信方式，且使用于任意组件间的通信

多个组件共享同一个状态（数据）

在 vue2 中只能用 vuex3 ，在 vue3 中只能用 vuex 4

**vuex工作原理**

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220911180306.png)

Vuex 由三个组成
* Actions：对象类型，存储动作
* Mutations：对象，修改加工与维护
* State：状态（数据），是一个对象，数据交由此对象来管理

三个对象都由`store`管理

在组件中通过store的 `dispatch(动作类型String, 值)` 此时在 `Actions` 中有符合动作类型的函数，调用此函数，在此函数中需要自己调用 `commit` 函数，在`Mutations` 中也会有对应的动作类型的函数，调用此函数会获取两个值：`state`与传入的`值`。最后vuex更新的数据会重新渲染到页面上

当某个动作**需要发起ajax请求**才能获取到数据，就需要在`Actions`中发起请求（如果不需要，是可以直接调用`commit`跳过`Actions`的）

一般的如果没有业务需求，都可以直接跳过`Actives`直接提交给`mutations`

**搭建开发环境**

* 1、在 `src` 目录下创建 `vuex` 目录，创建`store.js`用于创建和配置`store`

**import 导入会执行一遍，所以会造成在use之前就创建了`store`实例（不允许），所以需要在`store.js`中use**

```js
// 创建 vuex 中的 store
import Vuex from "vuex"
import Vue from "vue"

Vue.use(Vuex)

const actions = {}  
const mutations = {}  
const state = {}  
  
// 创建并导出  
export default new Vuex.Store({  
    actions,  
    mutations,  
    state  
})
```

* 2、引入vuex并让所有的vc能够访问vuex上的store，才能调用api


```js
// main.js
import store from '@/vuex/store'

// Vue.use(vuex)   // 必须在创建 store 实例对象之前，所以不能放在 main.js

new Vue({  
    render: h => h(App),  
    store  
}).$mount('#app')
```

**使用**

- 1、将数据放到 `store.js` 中的 `state` 对象上

```js
const state = {  
    count: 0,  
}
```

- 2、在页面获取数据

在 vc 上的 `$store` 上可以访问到 `state` 对象

```html
<h1>当前求和 {{this.$store.state.count}}</h1>
<select v-model.number="temp">  
  <option value="1">1</option>  
  <option value="2">2</option>  
  <option value="3">3</option>  
</select>  
<button @click="add">+</button>
```

- 3、在页面合适的时机调用 `dispatch()` 方法

```js
methods: {  
  add() {  
    // this.count += this.temp  
    this.$store.dispatch('add', this.temp)    // 这里的类型在 actives 中必须存在
  },
}
```

- 4、在 `store.js` 中配置对应的类型

```js
const actions = {  
    add(context, value) {     // 接收上下文参数也可以称为 miniStore 有一部分 Store 方法 
        context.commit('ADD', value)   // 这里的类型在 mutations 中必须存在
    }  
}  

const mutations = {  
    ADD(state, value) {   // 接收第一个为 state 对象
        state.count += value  
    }  
}
```

完整

```js
// store.js
const actions = {  
    add(context, value) {     // 没有什么业务可以不要
        context.commit('ADD', value)  
    },  
    addOdd(context, value) {  
        if (context.state.count % 2) {  
            context.commit('ADD', value)  
        }  
    },  
    addWait(context, value) {  
        setTimeout(() => {  
            context.commit('ADD', value)  
        }, 500)  
    },  
    de(context, value) {    // 没有什么业务可以不要
        context.commit('ADD', -value)  
    }  
}  
const mutations = {  
    ADD(state, value) {  
        state.count += value  
    }  
}  
const state = {  
    count: 0,  
}
```

```vue
// App.vue
<template>  
  <div>  
    <h1>当前求和 {{Sum}}</h1>  
    <select v-model.number="temp">  
      <option value="1">1</option>  
      <option value="2">2</option>  
      <option value="3">3</option>  
    </select>  
    <button @click="add">+</button>  
    <button @click="d">-</button>  
    <button @click="de">为奇数加</button>  
    <button @click="deng">等等加</button>  
  </div>  
</template>  
  
<script>  
  
export default {  
  name: "App",  
  data() {  
    return {  
      temp: 1,  
    }  
  },  
  methods: {  
    add() {  
	  // 可以跳过 Actives 直接提交给 mutations 
	  // this.$store.commit('ADD', this.temp)
      this.$store.dispatch('add', this.temp)  
    },  
    d() {  
      // this.$store.commit('ADD', -this.temp)
      this.$store.dispatch('de', this.temp)  
    },  
    de() {  
      this.$store.dispatch('addOdd', this.temp)  
    },  
    deng() {  
      this.$store.dispatch('addWait', this.temp)  
    }  
  },
  computed: {
	  Sum() {
		  return this.$store.state.count
	  }
  }
}  
</script>
```

注意：由于`actives`中的`context`参数是包含`dispatch`的，当一个`active`无法完成，可以在此`active`中调用`dispatch`传给下一个`active`直到业务完成，就可以在最后的`active`中`commit`

**store中的getters**

同样是一个对象，有点类似与[计算属性](#计算属性)，是对state的进一步加工

```js
// 用于将state中的数据进行加工  
const getters = {  
    bigSum(state) {  
        return 10 * state.count  
    }  
}

// 获取
this.$store.getters.bigSum
```

**mapState**

在组件中每次获取`state`中的数据都要通过`this.$store.state`的方式获取，代码非常臃肿，可以通过`vuex`提供的`mapState`方法来自动生成计算属性与`state`中数据的对应关系，只需要传入计算属性名字和`state`中对应的数据名字即可。需要注意，这中方法添加的会被开发者工具认为是`vuex bindings`，自己写的会被认为是`computed`

对象写法
```js
computed: {  
  // Sum() {  
  //   return this.$store.state.count  
  // }  
  
  // 对象写法
  ...mapState({Sum: 'count'})   // 如果有多个，可以直接在对象中配置多个键值对的对应关系
  // 数组写法
  ...mapState(['count']),   // 当名字相同时才可以，否则只能使用对象的写法
}
```

ES6语法，在对象中`...{}`相当于将这个`...{}`中的元素加到这个对象中

数组中的写法意味着一个元素两个用法，第一个作为计算属性名字，第二个作为从`state`中取名为此元素值

除了 `mapState` vuex中还有 `mapgetter、mapActives、mapMutations`，都是用于生成对应关系的函数，这些都是分批导出，所以导入时需要 `import {...} from xxx`


**mapGetters**

同mapState一样，mapGetters是为了自动生成`this.$store.getters`开头的访问

```js
computed: {  
	// bigSum() {  
	//   return this.$store.getters.bigSum  
	// }  
	...mapGetters(['bigSum']) // 数组写法
}
```

**mapMutations**

自动生成方法，调用`this.$store.commits()`联系`Mutations`

```js
methods: {  
  // add() {  
  //   this.$store.commit('add', this.temp)  
  // },  
  // d() {  
  //   this.$store.commit('add', -this.temp)  
  // },
    
  ...mapMutations({add: 'add', d: 'add'}), // 对象写法
}
```

**值得注意的是这样生成的代码片段如下，所以需要自己在调用函数时传入参数**

```js
add(value) {  
  this.$store.commit('add', value)  
},  
```

```html
<button @click="add(temp)">+</button>   // 调用函数时传参
```


**mapActions**

自动生成调用`this.$store.dispatch`，联系`Actions`

```js
methods: {  
	// de() {  
	//   this.$store.dispatch('addOdd', this.temp)  
	// },  
	// deng() {  
	//   this.$store.dispatch('addWait', this.temp)  
	// },
	...mapActions(['addOdd', 'addWait'])
}
```

与`mapMutations`相同，此时也需要在调用函数时传参

##### vuex的模块化

当数据过多，代码过于臃肿，服务于不同组件的数据与`active`不利于区分，就需要模块化。在一个模块中获取的所有都是这个模块里局部的

将用于一套功能（人员列表、求和等）的`Actives、Mutations、State、Getters`封装到一个对象中，然后在`Store`中使用`modules`配置项配置多组

这些模块配置都可以单独写入一个js文件中，方便后期维护，与代码的精简

```js
const addOptions = {  
    actions: {  
        addOdd(context, value) {  
            if (context.state.count % 2) {  
                context.commit('add', value)  
            }  
        },  
        addWait(context, value) {  
            setTimeout(() => {  
                context.commit('add', value)  
            }, 500)  
        },  
    },  
    mutations: {  
        add(state, value) {  
            state.count += value  
        }  
    },  
    state: {  
        count: 0  
    },  
    getters: {  
        bigSum(state) {  
            return 10 * state.count  
        }  
    }  
}  

const orderOptions = {
	actions: {},
	mutations: {},
	state: {},
	getters: {}
}
  
// 创建并导出  
export default new Vuex.Store({  
    modules: {  
        addOptions,
        orderOptions  
    }  
})
```

如上如果要获取的是`state`中的`addOptions`的属性，那么就需要新增一个参数，来指定获取的属性的来源

```js
...mapState('addOptions', ['count']),  // 规定从那个获取
...mapState(['addOptions']),   // 直接获取，这样在使用时需要使用 . 运算符 addOptions.count 形式
```

如果不借助`map...`方法，自己编写，需要如下

```js
this.$store.dispatch('addOptions/add', data)   // 在选择 actions 时以 命名空间/类型

this.$store.commit('addOptions/add', data)

this.$store.state.addOptions.count   // 多加一个命名空间

this.$store.getters['addOptions/bigNum']  // 注意getters分类不同于state，点运算符不支持 /
```

**在actions中请求后端接口**

由于数据是来源于后端接口，所以在`dispatch`时不需要传参

```js
getjuzi(context) {  
    axios.get('https://api.uixsj.cn/hitokoto/get?type=social').then(   // 精神小伙语录
        res => {  
            context.commit('getjuzi', res.data)  
        },  
        err => {  
            alert(err.message)  
        }  
    )  
}
```

#### ref引用

ref 是 vue 用来辅助开发者不依赖于 jQuery 来获取 DOM 元素或组件的引用（每个 vue 的组件实例都包含一个 `$refs` 对象，默认情况下指向空对象）

```vue
<template>
  <div>
    <h1>App.vue</h1>
    <button @click="getRefs">获取 refs</button>
  </div>
</template>
  
<script>
export default {
  name: "MyApp",
  methods: {
    getRefs() {
      console.log(this.$refs);
    },
  },
};
</script>
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220620011856.png)

**使用 ref 获取 DOM 元素**

* 为 DOM 元素指定 ref 属性和名称
* 通过 `this.$refs.名称` 的形式获取到 DOM 的引用

```vue
<template>
  <div>
    <!-- 指定属性 ref 的值 -->
    <h1 ref="h1">App.vue</h1>
    <button @click="getRefs">获取 refs</button>
  </div>
</template>
  
<script>
export default {
  name: "MyApp",
  methods: {
    getRefs() {
	  // 更改属性值为 h1 的 DOM 元素的样式
      this.$refs.h1.style.backgroundColor = "red";
    },
  },
};
</script>
```

**使用 ref 引用组件实例**

在使用组件A中使用组件B时，如果想在A中直接使用B中组件的实例对象，从而使用 `methods` 节点下的方法等

```vue
<!-- 使用 ref 属性，为组件添加引用名称 -->
<my-counter ref="counterRef"></my-counter>
<button @click="getRef">获取组件实例对象</button>

<script>
export default {
	methods: {
		getRef() {
			// 通过 this.$refs.引用名称 的方式可以获取到引用组件的实例

			// 使用引用组件中的 add() 方法
			this.$refs.counterRef.add();  // this.$refs.counterRef 获取的是组件实例对象
		}
	}
}
</script>
```

**$nextTick(cb)**

当涉及到 DOM 元素的更新，组件是异步执行 DOM 更新操作的，所以可能会出现通过 ref 获取到的 DOM 对象为 `undefined` ，此时需要 `ref.$nextTick(callback)` 方法使需要的获取 DOM 对象的操作推迟到下次 DOM 更新之后

```vue
<input type="text" v-if="inputVisible" ref="ipt">
<button v-else @click="showInput">展示输入框</button>

<script>
export default {
	methods: {
		showInput() {
			// 此时 input 元素会更新，为异步操作
			this.inputVisible = true;
			// 如果后续需要获取 DOM 元素，那么需要使用 $nextTick(cb) 推迟操作，负责页面上是没有此 DOM 元素的
			this.$nextTick(() => {
				// 获得焦点
				this.$refs.ipt.focus()
			})
		}
	}
}
</script>
```

#### 动态组件

动态组件指动态切换组件的显示和隐藏。vue 提供了一个内置的 `<component>` 组件，提供动态渲染
* `<component>` 是组件的占位符
* 通过 is 属性动态指定要渲染的组件的名称（不会触发 mounted 生命周期函数，只有 created）
* `component is="要渲染的组件"></component>`

```vue
<template>
  <div>
    <h1> App.vue</h1>
    <button @click="comName='MyHome'">首页</button>
    <button @click="comName='MyMovie'">电影</button>

	<!-- 由于是属性的绑定，所以需要 v-bind 命令 -->
    <component :is="comName"></component>
  </div>
</template>
  
<script>
import MyHome from "./components/active_component/Home.vue";
import MyMovie from "./components/active_component/Movie.vue";
  
export default {
  name: "MyApp",
  data() {
    return {
      // 声明变量，用于显示
      comName: 'MyHome'
    };
  },
  components: {
    MyHome,
    MyMovie,
  },
};
</script>
```

**使用 keep-alive 保持状态**

当动态的渲染组件时，组件中的状态变化是不会默认保存的，需要使用 keep-alive 保持状态

```vue
<keep-alive>
	<component :is="comName"></component>
</keep-alive>
```

#### 插槽

插槽（Slot）是 vue 为组件封装者提供的，允许在封装组件时，把不确定的、希望由用户指定的部分定义为插槽，为用户预留的占位符

通过 `<slot>` 元素定义插槽（如果没有预留，用户传入的自定义内容都会被抛弃）

```vue
<!-- Slot.vue -->
<template>
  <div>
    <h3>MySlot.vue</h3>
    <p>1</p>
	<!-- 插槽定义 -->
	<slot></slot>
    <p>2</p>
  </div>
</template>

<!-- App.vue -->
<template>
  <div>
    <h1>App.vue</h1>
    <!-- 在使用组件时，直接在标签内部填写内容，最后会渲染到 <slot> 的位置 -->
    <my-slot>
      <p>Slot</p>
    </my-slot>
  </div>
</template>
```

**后备内容**

封装组件时，可以为预留的 `slot` 插槽提供后备内容（默认内容），当用户没有提供自定义内容时，显示后备内容

```vue
<slot>
	<p>后备内容</p>
</slot>
```

**具名插槽**

如果需要预留多个插槽节点，则需要为每个插槽指定具体的 `name` 名称（如果没指定name属性，默认name="default"），如果要特定的渲染，需要在 `<template>` 标签使用 `v-slot` 指令（可以用 `#`替代 `v-slot:` ，而且只能在`template`标签写`v-slot`指令），包裹对应的内，或者在需要的标签上添加属性`slot="name"`

```vue
<!-- Slot.vue -->
<template>
  <div>
    <h3>MySlot.vue</h3>
    <div>
      <slot name="header"> </slot>
    </div>
    <hr />
    <div>
	  <!-- name="default" -->
      <slot> </slot>
    </div>
    <hr />
    <div>
      <slot name="footer"> </slot>
    </div>
  </div>
</template>

<!-- App.vue -->
<my-slot>
  <template #header>
	<h1>滕王阁序</h1>
  </template>

  <p>豫章故郡，洪都新府</p>
  <p>星分翼轸，地接衡庐</p>
  <p>襟三江而带五湖，控蛮荆而引瓯越</p>
  <!-- : 前后不要有空格 -->
  <template v-slot:footer>
	<p>王勃</p>
  </template>
</my-slot>
```

**作用域插槽**

为预留的 slot 插槽绑定 props 数据（作用域插槽），作用域插槽的数据都在子组件中

作用域插槽的数据是由组件提供的，而数据的渲染结构是由使用的组件（父组件）来决定

```vue
<!-- Slot.vue -->
<!-- 使用属性绑定 -->
<slot name="self" :info="info" :msg="msg"></slot>

<script>
export default {
  name: "MySlot",
  data() {
    return {
        info: {
            name: "MySlot",
            age: 18,
        },
        msg: "abc"
    }
  }
};
</script>

<!-- App.vue -->
<!-- 一般情况下都使用 scope 接收数据 -->
<template #self="scope">
	<!-- scope = { "info": { "name": "MySlot", "age": 18 }, "msg": "abc"} -->
	<p>作用域 {{scope.info.name}} {{scope.info.age}}</p>
</template>
```

解构作用域插槽的 prop

当scope中有多个属性，可以直接指定需要的属性

```vue
<template v-slot:self="{info, msg}">
	<p>作用域 {{info.name}} {{msg}}</p>
</template>
```

使用场景：
当需要渲染数据，此时为了让用户来决定渲染的样式、结构等，可以将此处预留作用域插槽，将数据以props的方式传递给用户，来实现用户对样式的自定义

#### 自定义指令

vue 内置的指令如果无法满足需求，可以自定义指令（不允许小驼峰）
* 私有自定义指令
* 全局自定义指令

推荐使用 `-` 连接

**指令函数(简写)调用时机**
* 指令与元素成功绑定（并非在页面上）
* 指令所在的模板被重新解析时

```html
<body>  
<div id="app">  
    <h2 v-mytext="n">{{n}}</h2>   <!-- 模拟实现 v-text -->    
    <input type="text" v-my-bind="n"/>      <!-- 模拟实现 v-bind --></div>  
  
<script>  
    Vue.config.productionTip = false;  
  
    new Vue({  
        el: "#app",  
        data: {  
            n: 1,  
        },  
        directives: {  
            // 接收两个参数  使用此指令的真实的 DOM 元素  对象，包含参数的值、定义的指令名字、使用指令名字等  
            mytext(element, binding) {  
                element.innerText = binding.value;  
            },  
            "my-bind": function(element, binding) {  
                element.focus();   // 获取焦点（初次加载失败）
                element.value = binding.value;  
            }  
        }  
    });  
</script>  
</body>
```

vue会先绑定元素和指令，然后才会渲染页面，故第一次调用时不会focus失效

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220902105744.png)

可以使用**对象形式**

```js
Vue.config.productionTip = false;  

new Vue({  
	el: "#app",  
	data: {  
		n: 1,  
	},  
	directives: {  
		"my-bind": {  // 常用的三个回调函数
			// 绑定时调用  
			bind(element, binding) {  
				element.value = binding.value;  
			},  
			// 元素插入页面调用  
			inserted(element, binding) {  
				element.focus();  
			},  
			// 指令所在模板被重新解析调用  
			update(element, binding) {  
				element.value = binding.value;  
			}  
		}  
	}  
});  
```


>指令中的所有 `this` 都是 `window` 不受 `vue` 管理

***如果在指令中需要获取指向vc 的this，那么就需要使用第三个参数`vnode`，此参数提供`context`属性可以获取this***

```js
new Vue({  
	el: "#app",  
	data: {  
		n: 1,  
	},  
	directives: {  
		fore(el, binding, vnode) {
			console.log(vnode.context)    // vc
		}
	}  
}
```

**声明私有自定义指令**

在组件的 `directives` 节点下声明私有自定义指令 `v-focus` ，在定义时不用加上 `v-` 使用时必须加上

```vue
<input type="text" v-focus />

<script>
export default {
	directives: {
		// 定义私有指令 focus
		focus: {
			// 当被绑定的元素插入到 DOM 中自动触发 mounted 函数
			mounted(el) {
				el.focus()   // 让当前元素获取焦点
			}
		}
	} 
}
</script>
```

**声明全局自定义指令**

在 `main.js` 中 使用 `app.directive()` 声明

```js
// Vue对象实例 app
app.directive('focus', {
    mounted(el) {
        el.focus();
    }
})
```

**updated 函数**

与 `mounted` 不同的是，`mouned` 只在第一次插入的时候调用，而 `updated` 在每次 DOM 更新完调用

```vue
focus: {
	updated(el) {
		el.focus();
	}
}
```

同时，此时的 mouned 和 updated 逻辑相同，那么可以不接收对象，而是直接指定函数

```js
app.directive('focus', (el) => {
    el.focus();
})
```

**指定参数值**

在绑定指令时，可以使用赋值符号为指令绑定具体的参数

```vue
<!-- MyOrder.vue -->
<p v-color="'blue'">MyOrder {{ count }}</p>
<input type="text" v-focus v-color="'red'" />

```

`binding.value` 就是指定的值

```js
<!-- main.js -->
app.directive('color', (el, binding) => {
    el.style.color = binding.value
})
```

#### 过渡与动画

```vue
<template>  
  <div>  
    <button @click="flag = !flag">显示隐藏</button>  
    <transition name="goo" appear>     <!-- appear 属性控制是否刚开始就加载动画 -->
      <h1 v-show="flag">goo</h1>  
    </transition>  
  </div>  
</template>  
  
<script>  
export default {  
  name: "Test",  
  data() {  
    return {  
      flag: true  
    }  
  },  
}  
</script>  
  
<style scoped>  
  h1 {  
    background-color: orange;  
  }  
  
  /* vue 封装的，规定的类名 默认为 v-enter-active 如果规定了 name 属性，就需要将 v 替换为 name */  
  .goo-enter-active {     
    animation: tran 1s;  
  }  
  
  .goo-leave-active {    /* 离场动画 */
    animation: tran 1s reverse;  
  }  
  
  /* 动画 */  
  @keyframes tran {  
    from {  
      transform: translateX(-100%);  
    }  
    to {  
      transform: translateX(0);  
    }  
  }  
</style>



```

需要动画的元素用 `<transition>` 标签包裹（只能包裹一个元素，如果有多个要相同效果，要么全部包裹到一个box中，要么用`<transition-group>`包裹并且给每一个元素都取唯一的属性 `key` ）

除了上述vue添加的类，还会添加如下的类（`name="goo"`）

```css
/* 进入的起点、离开的终点 */
.goo-enter, .goo-leave-to {}

/* 入场动画和出场动画 */
.goo-enter-active, .goo-leave-active{}

/* 进入终点、离开的起点 */
.goo-enter-to, .goo-leave {}
```

可以使用第三方库的css动画例如：`animate.css`

更改`name` 属性为第三方库规定的名字，在`<transition>`标签下的`enter-active-class`属性配置进入动画的类，在`leave-active-class`配置离开的动画
