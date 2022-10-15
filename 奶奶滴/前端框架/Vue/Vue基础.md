### vue 基础

#### vue 简介

vue是一套**用于构建用户界面的**前端**框架**

构建用户界面：为网站的使用者构建出美观、舒适、好用的网页
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220616005206.png)

传统的 Web 前端是使用 jquery + 模板引擎 的方式构建用户界面的

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220616005507.png)

使用vue构建用户界面
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220616010102.png)

框架：官方给 vue 的定位是前端框架，因为其提供了构建用户界面的一整套解决方案（vue全家桶）
* vue（核心库）
* vue-router（路由方案）
* vuex（状态管理方案）
* vue 组件库（快速搭建页面 UI 效果的方案）

辅助 vue 项目开发的一系列工具
* vue-cli（npm全局包：一键生成工程化的 vue 项目 - 基于webpack）
* vite（npm全局包：一键生成工程化的 vue 项目 -小巧）
* vue-devtools（浏览器插件：辅助调试的工具）
* vetur（vscode插件：提供语法高亮和只能提示）

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220616010810.png)
**vue特性**
* 数据驱动视图
	在使用了 vue 的页面中，vue 会监听数据的变化，从而自动重新渲染页面结构\
	![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220616011157.png)
	需要注意的是数据驱动视图是**单向的数据绑定**

* 双向数据绑定
	在填写表单时，双向数据绑定可以辅助开发者在不操作 DOM 的前提下，自动把用户填写的内容同步到数据源中
	![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220616011627.png)
* MVVM
	是 vue 实现数据驱动视图和双向数据绑定的核心原理，将html页面拆分为以下三部分
	![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220616011900.png)
	View      表示当前的页面渲染的 DOM 结构（模板）
	Model    表示当前页面渲染时依赖的数据源（data数据）
	ViewModel  是 vue 的实例，其是 MVVM 的核心（vue实例对象）
		![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220616012336.png)

#### vue的监测数据原理

案例
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
    <button @click="update">更新数据</button>  
    <ul>  
        <li v-for="item in list"  
            :key="item.id">  
            {{item.name}} - {{item.age}}  
        </li>  
    </ul>  
</div>  
  
<script>  
    Vue.config.productionTip = false;  
  
    const vm = new Vue({  
        el: "#app",  
        data: {  
            list: [  
                {id: "001", name: "马冬梅", age: "18"},  
            ],  
        },  
        methods: {  
            update() {  
                this.list[0] = {id: "001", name: "周杰伦", age: 18}  
            }  
        }  
    });  
</script>  
</body>  
</html>
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220901185933.png)

此时数据以及已经更改，但是 vue 并没有检测到数据发生了更改

**监测对象改变的原理**

每次发现数据改变，就调用 `setter` 模板顺带重新解析，然后生成虚拟 DOM 对比新旧虚拟 DOM渲染页面

核心原理

```js
// vue 实例对象
const vm = {};  

// 数据
let data = {  
    name: "jack",  
    age: 18  
}  

// 监视对象，监视data属性变化  (加工)
const obs = new Observer(data);  
  
function Observer(obj) {  
    // 汇总所有属性  
    const keys = Object.keys(obj);  
    // 遍历  
    keys.forEach((k) => {  
        Object.defineProperty(this, k, {  
            get() {  
                return obj[k]  
            },  
            set(val) {  
                obj[k] = val;  
            }  
        })  
    })  
}  
  
// 将监视对象赋值给 vm._data
vm._data = obs;
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220901193133.png)

使用 `Vue.set()` 添加响应式的数据（`_data`）

```js
Vue.set(target, key, value)

// 例如
Vue.set(vm.person, "sex", "男") 

// 在vue实例对象使用$set方法  效果相同
vm.$set(vm.person, "set", "男")
```

只能给data中的对象添加属性（对象不能是vue实例），不能给data根添加属性

**检测数组的原理**

当数据是一个数组时，数组中的每一个元素不是响应式的（没有为每一个元素服务的 `getter、setter`），直接通过索引更改是无法实现的

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220901213927.png)

vue中实现监听数组的元素是通过==数组对象的可以修改数组中元素的方法==，才会被vue认为是修改了数组

-   `push()`
-   `pop()`
-   `shift()`
-   `unshift()`
-   `splice()`
-   `sort()`
-   `reverse()`

vue对这些方法进行了包装，这时调用的方法，不是array对象原型上的方法（会先调用原型对象（参考 [js](../JS.md) 构造函数）上的方法，然后重新解析模板），除了这些以外的过滤等返回新数组的方法，可以直接通过赋值数组的形式达到同样的效果


#### 数据代理

将`data`中的所有数据加工（变为响应式，附带模板重新解析），然后给`vm实例`的`_data`

`_data`中获取数据和设置数据都需要调用 `getter、setter`（数据劫持）

通过一个对象代理对另一个对象中的属性进行读 / 写操作

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220901190459.png)

#### vue 的指令

**指令**是 vue 为开发者提供的模板语法，同于辅助开发者渲染页面的基本结构

* **内容渲染**指令
	辅助开发者渲染DOM的元素的文本内容
	* v-text

```js
// 将username对应的值渲染到标签 p 中
<p v-text='username'><p>
```

注意：v-text会覆盖元素内默认的值

```html
<body>
	<div id="app">
		<p v-text="username"></p>
		<p v-text="gender">会被覆盖</p>
	</div>
	<script src="./lib/vue-2.6.14.js"></script>
	<script>
		const vm = new Vue({
			el: '#app',
			data: {   // data() {return {}}   函数式写法，必须return对象
				username: 'John',
				gender: 'Male',
			}
			// 在 data 写法中，箭头函数的this指向的是父组件的
		});
		// vm.$mount("#app");   // 替代 el
	</script>
</body>
```

* {{}}

插值表达式：相当于占位符，相比v-text不会覆盖默认文本内容

```html
<body>
	<div id="app">
		<p>name: {{username}}</p>
		<p>gender: {{gender}}</p>
	</div>
	<script src="./lib/vue-2.6.14.js"></script>
	<script>
		const vm = new Vue({
			el: '#app',
			data: {
				username: 'John',
				gender: 'Male',
			}
		});
	</script>
</body>
```

插值可以直接使用所有vue实例对象的属性，以及表达式（例如：`1+1`）

* v-html
	用于渲染包含html标签的数据

* 属性绑定指令
	* v-bind
		为元素的属性动态绑定属性值

```html
<body>
	<div id="app">
		<!-- js表达式 -->
		<input type="text" v-bind:placeholder="inputValue + '我'">
	</div>
		  
	<script src="./lib/vue-2.6.14.js"></script>
			
	<script>
	   const vm = new Vue({
			el: '#app',
			data: {
				inputValue: 'Hello Vue!'
			}
		});
	</script>
</body>
``` 


也可以不写`v-bind`，直接`:属性`表示动态绑定属性

{{}}和v-bind都可以使用js的表达式（四则、三元以及一些数组字符串的方法）
	
* v-bind 动态绑定样式：`:class=""` 
* `:class="classStr"`：用于更换不同的样式（样式不定）
	* `:class="styleName"`
* `:class="classArray"`：用于多个样式（数组中放的字符串值，否则将会在vue对象查找）
	* `:class="['styleName', .....]"`
* `:class="classObj"`：通过对象配置，布尔值决定是否使用
	* `:class"{styleName: bool, .....}"`

* 事件绑定指令
	* v-on
		监听事件：一般使用@代替v-on，给DOM元素绑定时间监听，处理的函数需要在methods节点声明

```html
<body>
<div id="app">
	<h3>count 的值为 {{count}}</h3>
	<!-- v-on:click 简写 @click -->
	<!-- 可以直接传参 使用:函数名(参数)形式, 如果同时需要使用event那么传参$event-->
	<button @click="add(2, $event)" v-text="Add"></button>
</div>

<script src="./lib/vue-2.6.14.js"></script>

<script>
	const vm = new Vue({
		el: '#app',
		data: {
			count: 0,
			Add: '+2'
		},
		methods: {
			// 处理函数
			add(value, e) {   // e表示事件参数  是原生的 DOM event
				// this指向当前的 vm 实例对象，可以获取到当前的vue实例
				this.count += value;
				event.target.style.backgroundColor = 'red';
			}
		}
	});
</script>
</body>
```


* 事件修饰符
	* .prevent：阻止默认行为（a标签的跳转、表单提交等）
	* .stop：阻止事件冒泡
	* .capture：以捕获模式触发当前事件处理函数
	* .once：只触发一次
	* .self：只有 event.target 是当前元素时被触发
	* .passive：事件默认行为立即执行，无需等待事件回调函数执行完毕
	
	示例：`@click.stop='func'`
	
* 常见 按键修饰符（可以直接用`.键码`）
	* .esc
	* .enter
	* .delete
	* .space
	* .tab（在keydown时就会转换焦点，所以需要在keydown触发回调）
	* .up
	* .down
	* .left
	* .right
* 系统修饰键
	* ctrl、alt、shift、meta（win/command）
	* 配合keyup：按下同时，按下其他键，释放其他键事件才会被触发
	* 配合keydown：正常触发事件
* 自定义别名
	`Vue.config.keyCodes.别名 = 键码;`
* 组合键
	* 在 event 中有 `altKey、ctrlKey、metaKey、shiftKey` 可以判断按键中是否有这些系统修饰键
	* 或者直接采用连续拼接的方式 `.alt.enter` 的方式 （`.shift.ctrl.alt.y`等等）

* 双向绑定指令
	* v-model

```html
<body>
<div id="app">
	<p>用户名 {{username}} </p>
	<input type="text" v-model="username" />
<!-- 更改选择框的值，也会同步更改city的值 -->
<select v-model="city">
	<option value="">请选择</option>
	<option value="1">北京</option>
	<option value="2">上海</option>
	<option value="3">广州</option>
	<option value="4">深圳</option>
	<option value="5">杭州</option>
</select>
</div>

<script src="./lib/vue-2.6.14.js"></script>

<script>
   const vm = new Vue({
		el: '#app',
	   <!-- 更改数据值，会同步更改html页面的值 -->
		data: {
			username: 'zs',
			city: ''
		},
	});
</script>
</body>
```


注意：只能联合表单使用（默认获取value数据`v-model === v-model:value`）
* 修饰符
	* .number：将用户输入值转为数值
	* .trim：过滤首位空白字符
	* .lazy：非实时更新，失去焦点时更新


* 条件渲染指令
	* v-if、v-show
		都可以辅助控制元素的显示与隐藏（`"布尔值"`)

```html
<body>
<div id="app">
	<button @click="flag = !flag">Toggle Flag</button>
	<p v-if="flag">v-if</p>
	<p v-show="flag">v-show</p>
</div>
		
<script src="./lib/vue-2.6.14.js"></script>
			
<script>
	const vm = new Vue({
		el: '#app',
		data: {
			// flag 控制元素的显示与隐藏
			flag: true,
		},
	});
</script>
</body>
```


>不同的是 v-if是动态的创建或移除 DOM 元素，而v-show是动态的添加或移除`display:none`样式，所以，在运行时，如果频繁切换条件，那么使用v-show，反之使用v-if

`template` 标签（不会改变DOM结构）可以和 `v-if` 联合使用

* v-else、v-else-if
	配合v-if使用

* 列表渲染指令
	* v-for
		基于数组来循环渲染相似的UI结构`(item, index) in items`形式，index可选

```html
<body>
	<div id="app">
		<ul>
			<li v-for="item in list">name：{{item.name}}, age：{{item.age}}</li>
		</ul>
	</div>
		
	<script src="./lib/vue-2.6.14.js"></script>
  
	<script>
		const vm = new Vue({
			el: '#app',
			data: {
				list: [{
					name: '张三',
					age: 18
				},
				{
					name: '李四',
					age: 20
				}
				]
			},
		});
	</script>
</body>
```
	
列表数据变化时，会默认复用已存在的 DOM 元素，但会导致有状态的列表（被勾选的多选框）无法被正确更新。使用key维护列表的状态

```html
<body>
	<div id="app">
		<div>
			<input type="text" v-model="name">
			<button @click="addNewUser">添加</button>
		</div>
		<ul>
			<li v-for="item in list" :key="item.id">
				<input type="checkbox" />
				name：{{item.name}}, age：{{item.age}}
			</li>
		</ul>
	</div>

	<script src="./lib/vue-2.6.14.js"></script>
	
	<script>
		const vm = new Vue({
			el: '#app',
			data: {
				list: [{
					name: '张三',
					age: 18,
					id: '1'
				},
				{
					name: '李四',
					age: 20,
					id: '2'
				}],
			},
			methods: {
				addNewUser() {
					this.list.push({
						name: this.name,
						age: 18,
						id: this.nextId++
					})
				}
			}
		});
	</script>
</body>
```


注意：key的值只能是字符串或数值、key的值必须唯一、 index当作key的值没有意义，index的值不唯一、一般情况下使用v-for就使用key、一般将数据项的id属性作为key的值

虚拟DOM对比算法（通过key比较，相同的复用，不同的新生成）
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220829100729.png)

* v-cloak
	* v-cloak没有值，可以再当网速过慢时，对于那些需要vue解析的模板，可以加上v-cloak，借助css `[v-cloak: {display: none;}]` 方式隐藏未解析模板，等vue加载完成会删除模板上的 `v-cloak` 

* v-once
	* v-once没有值，使模板只会再初次动态渲染后就视为静态的，后续变化不会再次渲染此标签（用于看到初始值）

* v-pre
	* v-pre没有值，可以跳过该节点的编译（可以对没有插值语法节点使用，加快编译）


#### 过滤器

在 3.x 中，过滤器已移除，且不再支持。取而代之的是，建议用方法调用或计算属性来替换它们。

过滤器：对要显示的数据进行特定的格式化后再显示，不更改原数据只是生成新数据

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
    <p>{{dataName | filterName}}</p>   <!-- 数据 | 过滤器 -->
    <!-- 可以接收参数，但默认第一个就是dataName -->
    <!-- 过滤器可以多个一起使用 {{data | filter1 | filter2 ...}} 
	    前一个的返回当后一个的输入
    -->
</div>  
  
<script>  
    Vue.config.productionTip = false;  
  
    new Vue({  
        el: "#app",  
        data: {  
            dataName: 1,  
        },  
        filters: {  
            filterName(value, count=1) {  
                // 处理  
                let newDataName = value + count;  
                return newDataName;  // 返回值会替代 {{}} 的数据
			}  
        }  
    });  
</script>  
</body>  
</html>
```

#### 生命周期

生命周期：生命周期回调函数（this指向是vue实例或组件实例）、生命周期钩子
 
|生命周期函数|执行时机|阶段|单周期执行次数|应用场景|
|:--:|:--:|:--:|:--:|:--:|
|`created`|组件(元素)在内存中创建完毕|创建阶段|1|发起 ajax 请求|
|`mounted`|组件初次渲染完毕(挂载完毕)|创建阶段|1|操作 DOM 元素|
|`updated`|组件被重新渲染完毕|运行阶段|0或多|-|
|`unmounted`|组件被销毁后（内存与页面）|销毁阶段|1|-|

除此之外还有一些其他的生命周期函数。参考[vue官方文档](https://v3.cn.vuejs.org/api/options-lifecycle-hooks.html#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%92%A9%E5%AD%90)

|生命周期函数|执行时机|阶段|单周期执行次数|应用场景|
|:--:|:--:|:--:|:--:|:--:|
|`beforeCreate`|组件在内存中创建之前|创建阶段|1|-|
|`beforeMount`|组件渲染到页面之前|创建阶段|1|-|
|`beforeUpdate`|组件被重新渲染之前|运行阶段|0或多|-|
|`beforeUnmount`|组件被销毁之前|销毁阶段|1|-|

案例：透明度切换
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
    <h2 :style="{opacity}">Hello World</h2>
</div>  
  
<script>  
    Vue.config.productionTip = false;  
  
    const vm = new Vue({  
        el: "#app",  
        data: {  
            opacity: 1,  
        },  
        mounted() {   // vue完成模板解析，并把真实DOM放入页面后调用  
            setInterval(() => {  
                this.opacity -= 0.01  
                if (this.opacity <= 0) {  
                    this.opacity = 1;  
                }  
            }, 16)  
        }  
    });  
</script>  
</body>  
</html>
```

![](https://v2.cn.vuejs.org/images/lifecycle.png)


* Init Event & Lifecycle
	初始化：生命周期、事件，但数据代理未开始
	此时`beforeCreate`不能通过vue实例访问data数据和methods中的方法

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
	<h2>Hello World {{n}} </h2>   
</div>  
  
<script>  
	Vue.config.productionTip = false;  
  
	new Vue({  
		el: "#app",  
		data: {  
			n: 1,  
		},   
		beforeCreate() {  
			console.log(this)  // 没有 _data 属性  
			debugger;  // 断点调试  
		}  
	});  
</script>  
</body>  
</html>
```

* Init injections & reactivity
	初始化：数据监测、数据代理
	此时`created`可以通过vm访问data中的数据、methods中的方法
	
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
	<h2 :style="{opacity}">Hello World</h2>   
</div>  
  
<script>  
	Vue.config.productionTip = false;  
  
	const vm = new Vue({  
		el: "#app",  
		data: {  
			opacity: 1,  
		},  
		mounted() {   // vue完成模板解析，并把真实DOM放入页面后调用  
			setInterval(() => {  
				this.opacity -= 0.01  
				if (this.opacity <= 0) {  
					this.opacity = 1  
				}  
			}, 16)  
		},  
		created() {  
			console.log(this)  // 有 _data 属性  
			debugger;
		}  
	});  
</script>  
</body>  
</html>
```

* Has 'el' option -> Compile template ……
	开始解析模板，生成虚拟 DOM 还不能在页面中显示
	会先查看有无 `el` 配置，然后查看 `template` 选项
	此时`beforeMount`，页面呈现的是未编译的 DOM 结构，对 DOM 操作全部失效
	
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
	<h2>Hello World{{n}}</h2>  
</div>  
  
<script>  
	Vue.config.productionTip = false;  
  
	new Vue({  
		el: "#app",  
		data: {  
			n: 1,  
		},  
  
		beforeMount() {  
			document.getElementsByTagName("h2").value = "1";  
			debugger;  
		}  
	});  
</script>  
</body>  
</html>
```

* create vm.$el and replace "el" with it
	将内存中的虚拟 DOM 转为真实 DOM 插入到页面中去（真实DOM在$el中保存一份）
	`mounted` 此时页面中呈现的为经过 Vue 编译的 DOM，一般在此时进行：开启定时器、发送请求、订阅消息、绑定自定义事件等==初始化操作==
* Virtual DOM re-render and patch
	根据新数据生成新的虚拟 DOM，最终页面更新，完成 Model -> View 过程
	`beforeUpdate` 此时新数据还没有同步到页面中
	`updated` 此时数据和页面保持同步
* when vm.$destroy() is called
	当调用销毁方法后，vue解绑实例的全部指令已经==自定义事件监听器==
	`beforeDestroy` 此时vm所有的指令、data、methods还处于可用（对数据的更改不会重新解析模板）。一般在此时：关闭定时器、取消订阅消息、解绑自定义事件等==收尾操作==
* Teardown watchers, child components and event listeners
	移除自定义事件监听以及子组件全部移除
	`destroyed` vue实例已移除

完整的透明度切换

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
    <h2 :style="{opacity}">Hello World</h2>  
    <button @click="stopTimer">停止</button>  
</div>  
  
<script>  
    Vue.config.productionTip = false;  
  
    new Vue({  
        el: "#app",  
        data: {  
            opacity: 1,  
        },  
        methods: {  
          stopTimer() {  
              this.$destroy()  
          }  
        },  
        mounted() {  
            this.timer = setInterval(() => {  
                this.opacity -= 0.01  
                if (this.opacity <= 0)  
                    this.opacity = 1  
            }, 16)  
        },  
        beforeDestroy() {  
            clearInterval(this.timer)  
        }  
    });  
  
</script>  
</body>  
</html>
```

除了以上的还有其余的三个生命周期钩子，两个存在于路由中，另一个是`nextTick`
