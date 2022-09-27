# vue
## webpack

#### 前端工程化

前端工程化：将前端所需额工具、技术、流程、经验等进行规范化、标准化

* 模块化（JS模块化、CSS模块化、资源模块化）
* 组件化（复用休闲游的UI结构、样式、行为）
* 规范化（目录结构规范化、编码规范化、接口规范化、文档规范化、Git分支规范化）
* 自动化（自动化构建、自动部署自动化测试）

目前主流的前端工程化解决方案
* [webpack](https://www.webpackjs.com/)
* [parcel](https://zh.parceljs.org/)

#### webpack 的基本使用

webpack 是前端工程化的解决方案，提供了有好的**前端模块开发支持**，以及**代码压缩混淆**、处理**浏览器端JS的兼容性**、**性能优化**等

---
**基本的测试项目**

首先在项目根目录下初始化：在终端中输入`npm init -y`，就会在根目录下有一个记录的JSON文件`package.json`，这个文件记录了项目的一些信息，包含导入的包

在根目录下创建src目录作为项目源文件的目录，并在根目录下`npm install jquery -S`来添加jquery到dependencies（项目开发到上线都用到的包）中去

最后在src中编写代码，测试如下的案例代码

index.html
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="./index.js"></script>
</head>
<body>
    <ul id="list">

    </ul>
</body>
</html>
```
index.js
```js
// 导入 jquery
import $ from 'jquery'

$(function () {
    for (let i = 0; i < 9; i++) {
        let li = '<li>第' + (i + 1) +  '个 li</li>';
        $('#list').append(li);
    }
    // 奇数
    $('li:odd').css('background-color', 'pink');
    // 偶数
    $('li:even').css('background-color', 'blue');
});
```

但是这样会在浏览器中报错误`Uncaught SyntaxError: Cannot use import statement outside a module`这是由于import是ES6导入语法不兼容的问题

解决方案：导入webpack`npm install webpack@5.42.1 webpack-cli@4.7.2 -D` D与S差不多，D是放到devDependencies下代表只在开发时使用的

配置webpage

在根目录下创建文件`webpack.config.js`并添加如下
```js
// 向外导出webpack的配置对象
module.exports = {
    mode: 'development' 
    // mode 用来指定构建模式 
    // development：开发阶段（不会压缩，时间段，空间大），production：上线（压缩混淆，时间长，空间小）
}
```

在package.json下的script中添加如下，
```JSON
{
"scripts": {
    // 注意：JSON中不能写注释
    // 添加dev脚本，可通过 npm run 名字（dev） 执行
    "dev": "webpack"  
    }
}
```

最后在终端中 `npm run dev`启动 webpack 进行项目的打包构建(有问题，可以使用管理员执行，解决权限问题)

成功后会在**根目录**下创建一个dist目录，下有`main.js`资源文件

将源代码带入的js换为导入main.js即可（main.js将jquery.js和index.js整合到一起）

此时就会发现问题圆满解决，功能实现

>在运行dev脚本，会在运行webpack之前查看配置文件mode的构建模式，决定是否压缩等

* webpack 4.x和5.x版本中，会有如下的默认约定  
    * 默认的打包入口文件为src -> index.js   (没有则报错)
    * 默认的输出文件路径为dist -> main.js   (没有则创建)

可以在`webpack.config.js`配置文件中修改

```js
const path = require('path')

module.exports = {
    // 指定处理的文件
    entry: path.join(__dirname, './src/index.js'),
    // 指定生成的文件
    output: {
        // 存放目录
        path: path.join(__dirname, './dist'),
        // 文件名
        filename: 'index.js' 
    }
}
```


#### webpack插件

* webpack-dev-server
    * 类似nodejs的nodemon工具
    * 当修改源代码会自动打包构建（不会在之前指定的位置，而是存放在内存中，不会直接在磁盘创建，提高速度）默认在项目根目录生成，但是是虚拟的，不可见的，但是可访问
    * 将dev脚本改为`webpack serve`
    * 要查看效果，需要按照命令中提示的默认url访问，而且要更改文件的导入路径为虚拟的文件

* html-webpack-plugin
    * webpack中的HTML插件（类似模板引擎插件）
    * 可以自定义index.html页面的内容

~~~JS
// webpack.config.js
const HtmlPlugin = require('html-webpack-plugin');

// 插件实例对象
const htmlPlugin = new HtmlPlugin({
    template: './src/index.html',   // 模板文件（原文件）
    filename: './index.html'  // 新文件路径
});

module.exports = {
    mode: 'development',
    plugins: [htmlPlugin],     // 通过 plugins 节点添加插件
}
~~~

**devServer节点**

在`webpack.config.js`配置文件中，可以通过devServer节点对webpack-dev-server插件进行更多的配置

示例：
~~~js
devServer: {
	open: true,   // 初次打包，自动打开浏览器
	host: '127.0.0.1',  // 访问的ip地址
	port: 8080,   // 端口
}
~~~

#### loader

![](../../markdown_img/Pasted%20image%2020220615125023.png)

webpack默认只能打包处理.js后缀文件，其他的文件就需要调用loader加载器才可以正常打包

loader加载器的作用：协助webpack打包处理特定的文件模块，如：css-loader、less-loader、babel-loader等

在webpack中一切皆模块，当在index.js中导入index.css模块时，需要loader加载器才能正常打包

示例（打包处理css文件）：

```js
// index.js
// 导入 jquery
import $ from 'jquery'
// 导入css
import './css/index.css'

$(function () {
    for (let i = 0; i < 9; i++) {
        let li = '<li>第' + (i + 1) +  '个 li</li>';
        $('#list').append(li);
    }
    // 奇数
    $('li:odd').css('background-color', 'pink');
    // 偶数
    $('li:even').css('background-color', 'red');
});
```

~~~css
/* index.css */

#list {
    list-style: none;
    padding: 0;
    margin: 0;
}
~~~

```js
// webpack.config.json

module.exports = {
    module: {
        // 不同模块对应的规则
        rules: [{ 
            // 匹配的规则
            test: /\.css$/,
            // 使用的方法  从后往前，发现没有了就将更改的数据返回回去
            use: ['style-loader', 'css-loader'] 
        }]
    }
}
```

>在使用打包less时需要安装less和less-loader，在loader规则声明时不用写less，less为内置的

**打包url路径**

如果在样式或者js中有路径相关的则需要下载`url-loader`和`file-loader`同时`file-loader`也是内置的，不用声明，与less相同

当指定的url资源过多会影响资源的加载效率，此时可以在使用规则时用查询字符串的方式添加规则`?limit=INT`当图片资源不大于INT（单位byte）会转为base64格式

两种方法效果相同
~~~js
// webpack.config.js
{ test: /\.(jpg|png|gif)$/, use: ['url-loader?limit=2000'] },

{ test: /\.(jpg|png|gif)$/,
	use: {
		loader: 'url-loader',  // 指定loader
		options: {    // 指定属性
			limit: '2000'
		}
	}}
~~~

**打包高级js**

webpack只能打包处理一部分高级的js语法。对于无法处理的高级js语法可以借助于`babel-loader`进行打包处理

~~~sh
npm i babel-loader @babel/core @babel/plugin-proposal-class-properties -D
~~~

示例：

~~~js
// index.js
class Person {
	// 通过 static 关键字，为 Person 类定义了一个静态属性 info
	// webpack 无法打包 静态属性
	static info = 'person info'
}
~~~

~~~js
// webpack.config.js
{
	test: /\.js$/,
	// exclude 表示排除项，不处理 node_modules 中的 js 文件
	exclude: /node_modules/,
	use: {
		loader: 'babel-loader',
		options: {
			// 此插件用于转换 class 中的高级语法
			plugins: ['@babel/plugin-proposal-class-properties']
		},
	}
}
~~~


#### 打包发布

**原因**

项目完成之后，使用webpack对项目进行打包发布
* 开发环境下，打包生成的文件在内存中，无法获取最终打包生成的文件
* 开发环境下，打包生成的文件不会进程代码压缩和性能优化

**配置**

在package.json的script添加脚本

~~~json
"script": {
	"build": "webpack --mode production"    // 这里的mode会覆盖webpack配置文件中的mode
}
~~~

打包完成会在根目录下生成dist目录，储存所有的文件

如果想在dist中将不同类型的文件分类，可以在`webpack.config.js`中的output节点配置输出的路径（包含文件名）

~~~js
// js分类
output: {
	path: path.join(__dirname, './dist'),
	filename: 'js/index.js'
}
~~~

修改`url-loader`配置项，新增outputPath为对应的文件夹，实现对图片的分类

~~~js
use: {
	loader: 'url-loader',
	options: {
		limit: '2000',
		outputPath: 'images/',
	}
}},
~~~

自动清理dist的旧文件

安装`clean-webpack-plugin`插件

在`webpack-config.js`配置自动清除

~~~js
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const cleanPlugin = new CleanWebpackPlugin();
plugins: [
        htmlPlugin,
        cleanPlugin,
    ],
~~~

**企业级项目打包发布流程**

* 生成打包报告，根据报告分析具体的优化方案
* Tree-Shaking
* 为第三方库启用 CDN 加载
* 配置组件的按需加载
* 开启路由懒加载
* 自定制首页内容


#### Source Map

在对需要的js源代码进行压缩混淆，减少文件的大小提高文件加载效率时。不可避免的导致了对压缩混淆之后的代码debug非常困难

**Source Map**

Source Map 就是一个信息文件，里面储存这位置信息，就是代码压缩混淆前后的对应关系，在发生错误时，除错工具会根据Source Map 直接显示原始代码，方便后期调试

在开发环境下，webpack默认启用 Source Map 功能。可以直接在控制台提示错误信息的位置，并定位到具体源码

![](../../markdown_img/Pasted%20image%2020220616002105.png)

默认Source Map显示的转换后的代码，会导致行号和源文件不同，需要在配置文件中添加`devtool: 'eval-source-map'`属性

在生产环境中，如果省略 devtool 选项，则生成文件不包含 Source Map 能够防止源代码通过 Source Map 的方式泄露（会直接定位到压缩混淆的代码）

可以只定位行数，但不定位源代码，需要将`devtool`值设置为`nosources-source-map`

~~~js
// webpack.config.js
const path = require('path');
const HtmlPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

const htmlPlugin = new HtmlPlugin({
    template: './src/index.html',   // 模板文件
    filename: './index.html'  // 新文件名（内存中）
});

const cleanPlugin = new CleanWebpackPlugin();

// 向外导出webpack的配置对象
module.exports = {
    mode: 'development',
    // mode 用来指定构建模式
    // development：开发阶段（不会压缩，时间短，空间大），production：上线（压缩混淆，时间长，空间小）
    devtool: 'eval-source-map',  // 保证运行报错行数与源代码行数一致
    entry: path.join(__dirname, './src/index.js'),
    output: {
        path: path.join(__dirname, './dist'),
        filename: 'js/index.js'
    },
    // 通过 plugins 节点添加插件
    plugins: [
        htmlPlugin,
        cleanPlugin,
    ],
    // 指定模块如何被加载
    devServer: {
        port: 3000,
        host: 'localhost',
        open: true,  // 自动打开浏览器
    },
    // loader 配置
    module: {
        rules: [
            { test: /\.css$/, use: ['style-loader', 'css-loader'] },
            { test: /\.less$/, use: ['less-loader'] },
            {
                test: /\.(jpg|png|gif)$/,
                use: {
                    loader: 'url-loader',
                    options: {
                        limit: '2000',
                        outputPath: 'images/',
                    }
                }
            },
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        plugins: ['@babel/plugin-proposal-class-properties']
                    },
                }
            }
        ]
    },
}
~~~


---

## vue

### vue 基础

#### vue 简介

vue是一套**用于构建用户界面的**前端**框架**

构建用户界面：为网站的使用者构建出美观、舒适、好用的网页
![](../../markdown_img/Pasted%20image%2020220616005206.png)

传统的 Web 前端是使用 jquery + 模板引擎 的方式构建用户界面的

![](../../markdown_img/Pasted%20image%2020220616005507.png)

使用vue构建用户界面
![](../../markdown_img/Pasted%20image%2020220616010102.png)

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

![](../../markdown_img/Pasted%20image%2020220616010810.png)
**vue特性**
* 数据驱动视图
	在使用了 vue 的页面中，vue 会监听数据的变化，从而自动重新渲染页面结构\
	![](../../markdown_img/Pasted%20image%2020220616011157.png)
	需要注意的是数据驱动视图是**单向的数据绑定**

* 双向数据绑定
	在填写表单时，双向数据绑定可以辅助开发者在不操作 DOM 的前提下，自动把用户填写的内容同步到数据源中
	![](../../markdown_img/Pasted%20image%2020220616011627.png)
* MVVM
	是 vue 实现数据驱动视图和双向数据绑定的核心原理，将html页面拆分为以下三部分
	![](../../markdown_img/Pasted%20image%2020220616011900.png)
	View      表示当前的页面渲染的 DOM 结构（模板）
	Model    表示当前页面渲染时依赖的数据源（data数据）
	ViewModel  是 vue 的实例，其是 MVVM 的核心（vue实例对象）
		![](../../markdown_img/Pasted%20image%2020220616012336.png)

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

![](../../markdown_img/Pasted%20image%2020220901185933.png)

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

![](../../markdown_img/Pasted%20image%2020220901193133.png)

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

![](../../markdown_img/Pasted%20image%2020220901213927.png)

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

![](../../markdown_img/Pasted%20image%2020220901190459.png)

#### vue 的指令

**指令**是 vue 为开发者提供的模板语法，同于辅助开发者渲染页面的基本结构

* **内容渲染**指令
	辅助开发者渲染DOM的元素的文本内容
	* v-text
		~~~js
		// 将username对应的值渲染到标签 p 中
		<p v-text='username'><p>
		~~~
		注意：v-text会覆盖元素内默认的值
		~~~html
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
		~~~
	* {{}}
		插值表达式：相当于占位符，相比v-text不会覆盖默认文本内容
		~~~html
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
		~~~
		插值可以直接使用所有vue实例对象的属性，以及表达式（例如：`1+1`）
	* v-html
		用于渲染包含html标签的数据
* 属性绑定指令
	* v-bind
		为元素的属性动态绑定属性值
		~~~html
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
		~~~ 
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
		~~~html
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
		~~~
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
		~~~html
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
		~~~
	注意：只能联合表单使用（默认获取value数据`v-model === v-model:value`）
	* 修饰符
		* .number：将用户输入值转为数值
		* .trim：过滤首位空白字符
		* .lazy：非实时更新，失去焦点时更新


* 条件渲染指令
	* v-if、v-show
		都可以辅助控制元素的显示与隐藏（`"布尔值"`)
		~~~html
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
		~~~
		
	>不同的是 v-if是动态的创建或移除 DOM 元素，而v-show是动态的添加或移除`display:none`样式，所以，在运行时，如果频繁切换条件，那么使用v-show，反之使用v-if

	`template` 标签（不会改变DOM结构）可以和 `v-if` 联合使用

	* v-else、v-else-if
		配合v-if使用

* 列表渲染指令
	* v-for
		基于数组来循环渲染相似的UI结构`(item, index) in items`形式，index可选
		~~~html
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
		~~~
	
	列表数据变化时，会默认复用已存在的 DOM 元素，但会导致有状态的列表（被勾选的多选框）无法被正确更新。使用key维护列表的状态

	~~~html
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
	~~~
	注意：key的值只能是字符串或数值、key的值必须唯一、 index当作key的值没有意义，index的值不唯一、一般情况下使用v-for就使用key、一般将数据项的id属性作为key的值

虚拟DOM对比算法（通过key比较，相同的复用，不同的新生成）
![](../../markdown_img/Pasted%20image%2020220829100729.png)

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

~~~sh
# 初始化项目
npm init vite-app project_name
# 安装依赖包
cd project_name
npm install
# 启动项目
npm run dev
~~~

项目运行流程

在工程化的项目中，vue：通过 main.js 把 App.vue 渲染到 index.html 指定区域
* App.vue 用来编写带渲染的模板结构
* index.html 中需要预留一个el区域
* main.js 把 App.vue 渲染到 index.html 所预留的区域中

在.vue文件中，所有的模板都要写在`template`标签下

~~~vue
<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld msg="Hello Vue 3.0 + Vite" />
</template>
~~~

在main.js中进行渲染

~~~js
// createApp 创建 vue 的单页面应用程序实例
import { createApp } from 'vue'

// 导入待渲染的 App.vue 模板
import App from './App.vue'
import './index.css'

// 指定渲染的区域
createApp(App).mount('#app')
~~~

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

~~~vue
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
~~~

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

![](../../markdown_img/Pasted%20image%2020220903165315.png)

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


![](../../markdown_img/Pasted%20image%2020220903171026.png)

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


![](../../markdown_img/Pasted%20image%2020220903192557.png)

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

	![](../../markdown_img/Pasted%20image%2020220616211932.png)
	
	组件之间可以相互的引用（先注册后使用）
	* 全局注册组件：全局任何一个组件都可以使用
		![](../../markdown_img/Pasted%20image%2020220616212145.png)
		~~~js
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
		~~~

	* 局部注册组件：在当前注册的范围内使用
		![](../../markdown_img/Pasted%20image%2020220616212225.png)
		在需要的组件的script中注册（在components中添加，用标签的形式使用）
		~~~vue
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
		~~~
	
* 组件之间的样式冲突问题
	默认情况下在`.vue`组件中的样式会全局生效，导致很容易造成组件之间的样式冲突问题		
	例如：在父组件`App.vue`导入使用子组件`List.vue`时，更改父组件的样式，子组件的样式也会被更改
	解决：人为的为每个组件分配唯一的自定义属性，在编写样式时，通过属性选择器来控制样式的作用域
	~~~vue
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
	~~~

	但是这样非常的影响开发的效率，所以在style节点下提供了scoped属性，从而防止组件之间的样式冲突问题（会自动给每个DOM元素分配唯一的自定义属性）
	~~~vue
	<style scoped lang='less'>
	  p {
		  color: red;
	  }
	</style>
	~~~

* deep样式穿透
	对于父组件如果设置了scoped属性，那么其样式是不会对子组件生效（每一个样式会自动加上属性选择器），如果想要某些样式对子组件生效，可以使用`deep()深度选择器`
	~~~vue
	<style scoped lang='less'>
	.a :deep(.b) {
		// style
	}
	</style>
	~~~
			 
	示例：
	~~~vue
	<style lang='less' scoped>
	  p {
		color: red;
	  }
	  :deep(.title) {   // deep深度选择器
		color: blue;
	  }
	</style>
		~~~

**组件的 props**

为了提高组件的复用性，在封装 vue 组件时需要遵守日下规则：
* 组件的 DOM 结构、Style 样式尽量复用
* 组件中要展示的数据，尽量有组件的使用者提供

props 是组件的自定义属性，组件的使用者可以通过 props 把数据传递到子组件内部，供子组件内部进行使用

~~~vue
<!-- 通过自定义 props，把文章的标题和作者，传递到 my-article 组件中 -->
<my-article title="键盘敲烂，月入过万" author="Droliz"></my-article>
~~~

将动态的数据项声明为 props 自定义属性。自定义属性可以在当前的组件的模板结构中被直接使用

~~~vue
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
~~~

在使用自定义的属性时，可以使用`v-bind`指令传参，属性可以使用短横线命名法也可使用驼峰，与标签相同

**Class 与 style 绑定**

通过`v-bind`指令为元素动态绑定 class 属性的值和行内的 style 样式

class：可以使用三元表达式，动态的为元素绑定 class 的类名（简写 `:class="{类名: 布尔值}"`）

~~~vue
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
~~~

以数组的语法绑定（绑定多个，但是会导致语法臃肿）

~~~vue
<h3 class=".thin" :class="[isItalic ? 'italic' : '', isDelete ? 'delete' : '']"></h3>
~~~

以对象的语法绑定

~~~vue
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
~~~

以对象语法绑定内敛的style样式

~~~vue
<!-- 如果有短横线，要么改为驼峰，要么加引号代表字符串 -->
<div :style="{color: sctive, fontSize: fsize + 'px', 'background-color': bgcolor}">

data() {
	return {
		active: 'red',
		fsize: 30,
		bgcolor: 'pink'	
	}
}
~~~


#### props 验证

![](../../markdown_img/Pasted%20image%2020220617132221.png)

在封装组件时，对外界传递过来的 props 数据进行合法性校验，从而防止数据不合法问题

可以使用对象类型的 props

~~~vue
<School name="jack" :age="18"></School>

<script>
export default {
	props: {
		name: String,
		age: Number,
	}
}
</script>
~~~

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

~~~vue
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
~~~


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
~~~vue
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
~~~

相对于方法，计算属性会缓存计算的结果，只有依赖项发生变化时，才会重新进行运算。由此，计算属性的性能好（购物车的商品总个数和总计）

~~~vue
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
~~~

在计算属性中使用过滤器

~~~js
let a = 0
this.fruitlist
  .filter(x => x.state)   // 过滤器
  .forEash(x => {
    a += x.price * x.count
  })
~~~

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

~~~vue
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
~~~

**监听自定义事件**

在使用组件时，可以通过 `v-on` 的形式绑定自定义事件。

对于上述的 count 组件，在父组件 `App.vue` 中监听事件

~~~vue
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
~~~

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

![](../../markdown_img/Pasted%20image%2020220618062801.png)

当需要维护组件内外数据的同步时，也可以在组件上使用 `v-model` 指令

**父传子**

![](../../markdown_img/Pasted%20image%2020220618071340.png)

父组件通过 `v-bind:` 属性绑定的形式，把数据传递给父组件
子组件中通过 props 接收父组件传递过来的数据

**子传父**

![](../../markdown_img/Pasted%20image%2020220618071414.png)

在 `v-bind:` 指令之前添加 `v-model` 指令，使用自定义事件的形式将数据传递给父组件


**组件双向绑定数据的案例**

~~~vue
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
~~~


##### 任务列表案例

**最终效果**

![](../../markdown_img/Pasted%20image%2020220618195632.png)

**项目初始化**

使用 vite 初始化一个 SPA 项目

~~~sh
npm init vite-app project_list
~~~

安装依赖

~~~sh
npm install
~~~

安装 less 依赖

~~~sh
npm i less -D
~~~

项目的 src 目录结构

~~~txt
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

~~~

重置 `css` 全局样式

~~~css
:root {
  font-size: 12px;
}

body {
  padding: 8px;
}
~~~

重置 `App.vue` 组件

~~~vue
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
~~~

**封装 todo-list 组件**

1、创建并注册 TodoList 组件

在 `src/components/todo-list/` 目录下新建 `TodoList.vue` 组件

~~~vue
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
~~~

在 `App.vue` 组件中注册 `TodoList.vue` 组件

~~~vue
<!-- App.vue -->
<script>
import TodoList from './components/todo-list/TodoList.vue'
export default {
  components: {
    TodoList
  },
}
</script>
~~~

2、基于 bootstrap 渲染组件

使用bootstrap的[列表](https://v4.bootcss.com/docs/components/list-group/)和[复选框](https://v4.bootcss.com/docs/components/forms/)具体参考 bootstrap v4 官方文档

~~~vue
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
~~~

3、声明 `props` 属性

~~~vue
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
~~~

在 `App.vue` 组件中传参

~~~vue
<!-- App.vue -->
<template>
<todo-list :list="todoList"></todo-list>
</template>
~~~

4、渲染 DOM 结构

通过 `v-for` 渲染 DOM 结构

~~~vue
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
~~~

通过 `v-if` 和 `v-else` 选择渲染

~~~html
<!-- TodoList.vue -->
<span class="badge badge-success badge-pill" v-if="item.done">完成</span>
<span class="badge badge-warning badge-pill" v-else>未完成</span>
~~~

通过 `v-model` 双向绑定

~~~vue
<!-- TodoList.vue -->
<input type="checkbox" class="custom-control-input" :id="item.id" v-model="item.done"/>
~~~


通过 `v-bind` 动态绑定样式

~~~vue
<!-- TodoList.vue -->
<label class="custom-control-label" :class="{delete: item.done}" :for="item.id">
~~~

样式

~~~vue
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
~~~

**封装 todo-input 组件**

1、创建并注册 TodoInput 组件

在 `src/components/todo-input/` 目录下新建 `TodoInput.vue` 组件

~~~vue
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
~~~

在 `App.vue` 组件中注册 `TodoInput.vue` 组件

~~~vue
<!-- App.vue -->
<script>
import TodoInput from './components/todo-input/TodoInput.vue'
export default {
  components: {
    TodoInput,
  },
}
</script>
~~~

2、基于 bootstrap 渲染组件

~~~vue
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
~~~

3、通过自定义事件向外传递数据

添加的任务需要传入到 `App.vue` 中的todoList数组中去，使用自定义事件传递

声明数据

~~~vue
<!-- TodoInput.vue.vue -->
data() {
	return {
	  task: '',
	};
},
~~~

`v-model` 双向绑定数据

~~~vue
<!-- TodoInput.vue -->

<!-- 输入框进行v-model双向数据绑定 -->
<input
  type="text"
  class="form-control"
  placeholder="请填写任务"
  style="width: 356px"
  v-model.trim="task"    
/>
~~~

阻止表单默认提交，声明自定义事件，并指定事件处理函数

~~~vue
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
~~~

4、在 `App.vue` 组件中监听 `add` 自定义事件

~~~vue
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
~~~


**TodoButton 组件**

1、创建并注册 TodoInput 组件

在 `src/components/todo-button/` 目录下新建 `TodoButton.vue` 组件

~~~vue
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
~~~

在 `App.vue` 组件中注册 `TodoInput.vue` 组件

~~~vue
<!-- App.vue -->
<script>
import TodoButton from './components/todo-button/TodoButton.vue'
export default {
  components: {
    TodoButton,
  },
}
</script>
~~~

2、基于 bootstrap 渲染组件

~~~vue
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
~~~

3、通过 `props` 指定默认激活的按钮

在props中声明，默认值为 0，0：全部、1：已完成、2：未完成

~~~vue
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
~~~

给按钮动态绑定类名

~~~html
<!-- TodoButton.vue.vue -->

<button type="button" class="btn" :class="active === 0 ? 'btn-primary' : 'btn-secondary'">全部</button>
<button type="button" class="btn" :class="active === 1 ? 'btn-primary' : 'btn-secondary'">已完成</button>
<button type="button" class="btn" :class="active === 2 ? 'btn-primary' : 'btn-secondary'">未完成</button>
~~~

4、通过 `v-model` 更新激项的索引

>父  --->  子   通过 props 传递激活项的索引
>子  --->  父   更新父组件中的 activeBtnIndex

绑定按钮点击事件

~~~vue
<!-- TodoButton.vue.vue -->
<button type="button" class="btn" :class="active === 0 ? 'btn-primary' : 'btn-secondary'" @click="btnClick(0)">全部</button>
<button type="button" class="btn" :class="active === 1 ? 'btn-primary' : 'btn-secondary'" @click="btnClick(1)">已完成</button>
<button type="button" class="btn" :class="active === 2 ? 'btn-primary' : 'btn-secondary'" @click="btnClick(2)">未完成</button>
~~~

声明自定义事件，用来更新父组件通过 `v-model` 指令传递过来的 `props` 数据

~~~vue
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
~~~

通过计算属性动态更改列表

根据当前的 `App.vue` 中的 `activeBtnIndex` 值，选择性的向 `TodoList.vue` 传入需要的列表数据

在 `App.vue` 中使用计算属性

~~~vue
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
~~~



#### watch 侦听器

watch 侦听器允许开发者监视数据的变化，从而针对数据的变化做特定的操作（监视用户名的变化并发起请求，判断用户名是否可用） 相比计算属性，侦听器中可以进行异步操作（定时器等）

所有不被vue管理的函数（定时器回调、ajax回调）都写成箭头函数，this指向才是vue实例对象

**watch 侦听器基本语法**

在 watch 节点下，定义自己的侦听器

~~~vue
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
~~~

![](../../markdown_img/Pasted%20image%2020220618205838.png)
* immediate选项
	* 在默认情况下，初次渲染完成是不会调用侦听器的，如果需要初次就开始调用需要使用immediate属性
	~~~js
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
	~~~

* deep选项
	* 如果 watch 侦听的是对象，如果对象的属性值变化，watch是无法侦听到的
	~~~js
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
	~~~

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

~~~vue
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
~~~

#### 组件的生命周期

**组件的运行过程**

![](../../markdown_img/Pasted%20image%2020220619162439.png)

**监听组件的不同时刻**

vue 框架为组件内置了不同时刻的生命周期函数，会伴随组件的运行自动调用
* 组件在内存中创建完毕之后，会调用 created 函数
* 被成功渲染到页面上，会调用 mounted 函数
* 被销毁之后，会调用 unmounted 函数

~~~vue
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
~~~

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

![](../../markdown_img/Pasted%20image%2020220619200947.png)

使用 EventBus 实现（第三方包 mitt 中的 eventBus 对象）

数据的接收方通过 `.on` 方法注册自定义事件，数据的发送方通过 `.emit` 方法触发自定义事件，并发送数据

~~~js
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
~~~

**后代关系组件之间的数据共享**

父节点的组件向子孙组件共享数据，可以使用 `provide` 和 `inject` 实现

父节点的组件通过 `provide` 方法，对子孙组件共享数据

~~~vue
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
~~~

子孙节点通过 `inject` 方法，接收父组件共享的数据

~~~vue
<script>
export default {
	// 子孙组件使用 inject 接收共享的数据
	inject: ['color']
}
</script>
~~~

**父节点对外共享响应式数据**

在上述共享中，如果父级变化，子孙级不会响应式的变化，此时需要结合computed函数向下共享

此时子孙节点如果要使用该数据，需要采用 `数据名.value` 的形式

~~~js
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
~~~

**vuex**

vuex 是终极的组件之间的数据共享方案（多个组件依赖于同一个状态），可以让组件之间的数据共享变得 清晰、高效、且易于维护

![](../../markdown_img/Pasted%20image%2020220619223350.png)


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

![](../../markdown_img/Pasted%20image%2020220619232808.png)

~~~js
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
~~~


#### vuex

专门在 Vue 中实现集中式状态（数据）管理的一个 Vue 插件，也是一种组件间的通信方式，且使用于任意组件间的通信

多个组件共享同一个状态（数据）

在 vue2 中只能用 vuex3 ，在 vue3 中只能用 vuex 4

**vuex工作原理**

![](../../markdown_img/Pasted%20image%2020220911180306.png)

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
// 创建 vuex 中的 storeimport Vuex from "vuex"  
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

~~~vue
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
~~~

![](../../markdown_img/Pasted%20image%2020220620011856.png)

**使用 ref 获取 DOM 元素**

* 为 DOM 元素指定 ref 属性和名称
* 通过 `this.$refs.名称` 的形式获取到 DOM 的引用

~~~vue
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
~~~

**使用 ref 引用组件实例**

在使用组件A中使用组件B时，如果想在A中直接使用B中组件的实例对象，从而使用 `methods` 节点下的方法等

~~~vue
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
~~~

**$nextTick(cb)**

当涉及到 DOM 元素的更新，组件是异步执行 DOM 更新操作的，所以可能会出现通过 ref 获取到的 DOM 对象为 `undefined` ，此时需要 `ref.$nextTick(callback)` 方法使需要的获取 DOM 对象的操作推迟到下次 DOM 更新之后

~~~vue
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
~~~

#### 动态组件

动态组件指动态切换组件的显示和隐藏。vue 提供了一个内置的 `<component>` 组件，提供动态渲染
* `<component>` 是组件的占位符
* 通过 is 属性动态指定要渲染的组件的名称（不会触发 mounted 生命周期函数，只有 created）
* `component is="要渲染的组件"></component>`

~~~vue
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
~~~

**使用 keep-alive 保持状态**

当动态的渲染组件时，组件中的状态变化是不会默认保存的，需要使用 keep-alive 保持状态

~~~vue
<keep-alive>
	<component :is="comName"></component>
</keep-alive>
~~~

#### 插槽

插槽（Slot）是 vue 为组件封装者提供的，允许在封装组件时，把不确定的、希望由用户指定的部分定义为插槽，为用户预留的占位符

通过 `<slot>` 元素定义插槽（如果没有预留，用户传入的自定义内容都会被抛弃）

~~~vue
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
~~~

**后备内容**

封装组件时，可以为预留的 `slot` 插槽提供后备内容（默认内容），当用户没有提供自定义内容时，显示后备内容

~~~vue
<slot>
	<p>后备内容</p>
</slot>
~~~

**具名插槽**

如果需要预留多个插槽节点，则需要为每个插槽指定具体的 `name` 名称（如果没指定name属性，默认name="default"），如果要特定的渲染，需要在 `<template>` 标签使用 `v-slot` 指令（可以用 `#`替代 `v-slot:` ，而且只能在`template`标签写`v-slot`指令），包裹对应的内，或者在需要的标签上添加属性`slot="name"`

~~~vue
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
~~~

**作用域插槽**

为预留的 slot 插槽绑定 props 数据（作用域插槽），作用域插槽的数据都在子组件中

作用域插槽的数据是由组件提供的，而数据的渲染结构是由使用的组件（父组件）来决定

~~~vue
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
~~~

解构作用域插槽的 prop

当scope中有多个属性，可以直接指定需要的属性

~~~vue
<template v-slot:self="{info, msg}">
	<p>作用域 {{info.name}} {{msg}}</p>
</template>
~~~

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

![](../../markdown_img/Pasted%20image%2020220902105744.png)

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

**声明私有自定义指令**

在组件的 `directives` 节点下声明私有自定义指令 `v-focus` ，在定义时不用加上 `v-` 使用时必须加上

~~~vue
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
~~~

**声明全局自定义指令**

在 `main.js` 中 使用 `app.directive()` 声明

~~~js
// Vue对象实例 app
app.directive('focus', {
    mounted(el) {
        el.focus();
    }
})
~~~

**updated 函数**

与 `mounted` 不同的是，`mouned` 只在第一次插入的时候调用，而 `updated` 在每次 DOM 更新完调用

~~~vue
focus: {
	updated(el) {
		el.focus();
	}
}
~~~

同时，此时的 mouned 和 updated 逻辑相同，那么可以不接收对象，而是直接指定函数

~~~js
app.directive('focus', (el) => {
    el.focus();
})
~~~

**指定参数值**

在绑定指令时，可以使用赋值符号为指令绑定具体的参数

~~~vue
<!-- MyOrder.vue -->
<p v-color="'blue'">MyOrder {{ count }}</p>
<input type="text" v-focus v-color="'red'" />

~~~

`binding.value` 就是指定的值

~~~js
<!-- main.js -->
app.directive('color', (el, binding) => {
    el.style.color = binding.value
})
~~~

#### 过度与动画

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

![](../../markdown_img/Pasted%20image%2020220622152623.png)

#### vue-router 的基本使用

vue-router 是 vue 官方给出的路由解决方案（插件）。只能结合 vue 项目使用（3.x版本适用于vue2.x，4.x版本适用于vue3.x）。路由的数据需要通过 `ajax` 获取。被切换走的路由组件默认是被销毁了

安装 `vue-router 4.x` 

~~~sh
npm i vue-router -S   # vue3

npm i vue-router@3 -S   # vue2
~~~

使用 `<router-link>` 标签声明路由链接，并使用 `<router-view>` 标签声明路由占位符

~~~vue
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
~~~

每个路由组件都有两个属性`$route`（只读对象）和`$router`（只写对象），前者只存放自己的`router`对象，而第二个存放所有参与路由的组件。可以在组件中使用`thi.$route`获取组件路由的配置信息

**配置路由**

在项目中创建 `router.js` 路由模块，在其中创建并得到路由对象
* 从 vue-router 中按需导入两个方法
* 导入需要使用路由控制的组件
* 创建路由实例对象
* 向外共享路由实例对象
* 在 `main.js` 中导入并挂载路由模块

~~~js
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
~~~

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

~~~js
const routes = [  
    {path: '/', redirect: '/home'},  
    {path: '/home', component: Home},  
    {path: '/movie', component: Movie},  
    {path: '/about', component: About}  
]
~~~

**路由高亮**

通过两种方案实现对激活的路由进行高亮显示
* 使用默认的高亮class类
	被激活的路由链接，会默认引用在 `index.css` 中的一个 `router-link-active` 的类名

	~~~css
	/* 自定义 */
	.router-link-active {  
	    background-color: red;  
	    color: white;  
	    font-weight: bold;  
	}
	~~~
	
* 自定义路由高亮的class类
	在创建路由实例对象时，可以基于 `linkActiveClass` 属性，自定义路由连接被激活时应用的类名
	也可以在`route-link`标签中使用`active-class`属性指定样式

	~~~js
	const router = createRouter({  
	    // 指定工作模式  
	    history: createWebHashHistory(),  
	    // 通过 routes 数组，指定路由规则  
	    routes,  
	    // 指定路由被激活时应用的样式的类名  
	    linkActiveClass: 'router-active'  
	});
	~~~

**嵌套路由**

嵌套路由：通过路由实现组件的嵌套展示
* 声明子路由连接和子路由占位符

	~~~vue
	<!-- 在about中声明两个子路由 -->
	<template>  
	  <div>  
	    <p>MyAbout.vue</p>  
	    <router-link to="/about/tab1">tab1</router-link>  
	    <router-link to="/about/tab2">tab2</router-link>  
	      
	    <router-view></router-view>  
	  </div>  
	</template>
	~~~

* 在父路由的规则中，通过 `children` 属性嵌套声明子路由规则

	~~~js
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
	~~~

在嵌套路由中使用路由的重定向

~~~js
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
~~~

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

~~~js
{  
    path: '/movie/:id',   
    name: "mov",  
    component: Movie,   
{
~~~

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

~~~vue
<router-link :to="{ 
	name: 'mov', 
	params: { id: 3 } 
}">go to movie</router-link>
~~~

在使用 `this.$router.push()` 方法时，可以使用对象的方式指定

```js
this.$router.push({ name: 'mov', params: { id: 3 } });
```

**动态路由匹配**

动态路由：将 Hash 地址中可变的部分定义为参数项，从而提高路由规则的复用性（使用 `:` 来定义路由参数项）

~~~js
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
~~~

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


~~~vue
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
~~~


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


##### 路由守卫

路由守卫可以控制路由的访问权限

![](../../markdown_img/Pasted%20image%2020220623021738.png)

全局导航守卫：会拦截每个路由规则，对每个路由规则进行访问权限的控制

###### 全局守卫

**全局前置路由守卫**

初始化被调用，每次路由切换之前调用

~~~js
// router.js

const router =  new VueRouter({  
    routes  
})

router.beforeEach(() => {  
    console.log("触发守卫方法")
})
~~~

守卫方法的三个形参
* `to`：目标路由对象
* `from`：当前导航正要离开的路由对象
* `next`：函数，表示放行

~~~js
// router.js
router.beforeEach((to, from, next) => {  
    console.log(to)  
    console.log(from)  
    // 如果声明了 next 必须调用 next 负责不允许访问任何组件  
    next();  // 允许访问任何组件  
})
~~~

`next` 函数的三种调用方式

直接放行：`next()`
强制停留当前页面：`next(false)`
强制跳转到指定页面：`next('hash地址')`

结合 `token` 控制页面得访问权限

~~~js
router.beforeEach((to, from, next) => {  
    const token = localStorage.getItem('token')  // 读取 token    
    if (to.path === '/main' && !token) {  
        next('/login')   // 登录
    } else {  
        next()    // 放行
    }  
})
~~~

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

### vue 综合

#### vue-cli

**简介**

vue-cli （vue脚手架）是 vue 官方提供得、快速生成 vue 工程化项目的工具

`vue-cli` 基于webpack，支持vue2和vue3项目

**安装和使用**

~~~sh
# 全局安装
npm i -g @vue/cli

# 查看vue-cli版本
vue --version

# 查看版本不识别 vue 
# 管理员运行cmd
set-ExecutionPolicy RemoteSigned
Y   # 回车
~~~

创建一个 vue-cli 项目

~~~sh
vue create project_name   # 以命令行方式创建

vue ui   # 以 ui 界面的形式创建

# 启动项目
npm run server
# 打包项目（编译并压缩）
npm run bulid
~~~


~~~sh
# 如果在创建项目是终端报错
 ERROR  Failed to get response from Error: JAVA_HOME is incorrectly set.
	Please update XXXX

# 这是因为安装的 haddop 等其他的中的 yarn 和 npm 冲突

# 解决方案：在 C:\Users\lenovo\vuerc（vue的配置文件）中修改如下
{
  "packageManager": "npm",
  "useTaobaoRegistry": false,
}
~~~

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

~~~sh
npm i element-plus -S
~~~

注册 element-ui 为 vue 插件

~~~js
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
~~~

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

![](../../markdown_img/Pasted%20image%2020220623232850.png)

应用于：token身份验证、loading 效果 等

**配置请求拦截器**

通过调用 `axios.interceptors.request.use(成功回调，失败回调)` 失败回调可省略。成功回调可以接收 config 对象，而且必须将 config 返回，否则请求无法发起

~~~js
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
~~~

#### proxy 跨域代理

在项目遇到接口跨域的问题时，可以通过代理的方式解决
* 把 `axios` 请求的根目录设置为 vue 项目的运行地址（接口请求不在跨域）
* vue 项目发现请求的接口不存在，就会将请求转交给 `proxy` 代理
* 代理把请求的根路径替换为 `devServer.proxy` 属性的值，发起真正的数据请求
* 代理将请求过来的数据，转发给 axios

在项目根目录创建配置文件 `vue.config.js` 然后添加如下配置。重启项目即可

~~~js
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
~~~

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


## 示例

### 列表的显示

通过输入框的值，模糊查询显示列表

**通过函数实现**
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
    <input v-model="temp"/>  
    <ul>  
        <li v-for="item in list"  
            :key="item.id"  
            v-show="isShow(temp, item.name)">  
            {{item.name}} - {{item.age}}  
        </li>  
    </ul>  
</div>  
  
<script>  
    Vue.config.productionTip = false;  
  
    const vm = new Vue({  
        el: "#app",  
        data: {  
            temp: "",  
            list: [  
                {  
                    id: "001",  
                    name: "马冬梅",  
                    age: "18",  
                },  
                {  
                    id: "002",  
                    name: "周冬雨",  
                    age: "14",  
                },  
                {  
                    id: "003",  
                    name: "周杰伦",  
                    age: "20",  
                },  
            ],  
        },  
        methods: {  
            isShow(s, name) {  
                if (!s) {  
                    return true;  
                }  
                return name.indexOf(s) !== -1;  
            }  
        },  
    });  
  
</script>  
</body>  
</html>
```


**通过watch实现**
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
    <input v-model="temp"/>  
    <ul>  
        <li v-for="item in fillList"  
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
            temp: "",  
            list: [  
                {id: "001", name: "马冬梅", age: "18"},  
                {id: "002", name: "周冬雨", age: "14"},  
                {id: "003", name: "周杰伦", age: "20"},  
            ],  
            fillList: []  // 过滤的数据  
        },  
        watch: {  
            temp: {  
                immediate: true,  
                handler(newVal) {  
                    this.fillList = this.list.filter((obj) => {  // filter 过滤返回新数组  
                        return obj.name.indexOf(newVal) !== -1;  
                    })  
                }  
            }  
        }  
    });  
  
</script>  
</body>  
</html>
```


**通过计算属性实现（优先使用）**
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
    <input v-model="temp"/>  
    <ul>  
        <li v-for="item in fillList"  
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
            temp: "",  
            list: [  
                {id: "001", name: "马冬梅", age: "18"},  
                {id: "002", name: "周冬雨", age: "14"},  
                {id: "003", name: "周杰伦", age: "20"},  
            ],  
        },  
        computed: {  
            fillList() {  
                return this.list.filter((obj) => {  
                    return obj.name.indexOf(this.temp) !== -1;  
                });  
            }  
        }  
    });  
  
</script>  
</body>  
</html>
```

在上面基础上制作升序降序以及默认排序按钮

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
    <label>  
        <input v-model="temp"/>  
    </label>  
    <button @click="sortType = 0">升</button>  
    <button @click="sortType = 1">降</button>  
    <button @click="sortType = 2">原</button>  
  
    <ul>   <!-- 数据全部来源于 fillList 故关于数据的操作全部在 fillList 中完成 -->
        <li v-for="item in fillList"   
            :key="item.id">  
            {{item.name}} - {{item.age}}  
        </li>  
    </ul>  
</div>  
  
<script>  
    Vue.config.productionTip = false;  
  
    new Vue({  
        el: "#app",  
        data: {  
            temp: "",  
            list: [  
                {id: "001", name: "马冬梅", age: "18"},  
                {id: "002", name: "周冬雨", age: "24"},  
                {id: "003", name: "周杰伦", age: "20"},  
                {id: "004", name: "张一山", age: "26"},  
                {id: "005", name: "刘德华", age: "40"},  
            ],  
            sortType: 2,   // 0：升  1：降  2：默认  
        },  
        computed: {  
            fillList: {  
				get() {  
				    let tmp = this.list.filter((obj) => {  
				        return obj.name.indexOf(this.temp) !== -1;  
				    });  
				    switch (this.sortType) {  
				        case 0:  
				            tmp.sort((a, b) => a.age - b.age);  
				            break;  
				        case 1:  
				            tmp.sort((a, b) => b.age - a.age);  
				            break;  
				    }  
				    // 可以用三元代替
					// if (this.sortType !== 2) {  
					//     tmp.sort((o1, o2) => {  
					//         return this.sortType ? o2.age-o1.age : o1.age-o2.age;  
					//     });  
					// }
				    return tmp;  
				},
            }  
        },  
    });  
  
</script>  
</body>  
</html>
```


### v-model 收集表单数据

* `type="text"、"password"、"radio"` 时，获取的是value值，需要配置value
* `type="checkbox"` 时，如果没有配置value，当v-model初始值为非数组获取的是是否勾选bool，如果初始值为数组获取的是勾选的元素的数组

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
    <form @submit.prevent="submit">  
        <label><span>账号: </span><input type="text" v-model.trim="userInfo.account"/></label>  
        <br><br>  
        <label><span>密码: </span><input type="password" v-model.trim="userInfo.password"/></label>  
        <br><br>  
        <label><span>年龄: </span><input type="number" v-model.number="userInfo.age"/></label>  
        <br><br>  
        <span>性别: </span>  
        <label><span>男</span><input type="radio" name="sex" v-model="userInfo.sex" value="男"/></label>  
        <label><span>女</span><input type="radio" name="sex" v-model="userInfo.sex" value="女"/></label>  
        <br><br>  
        <span>爱好: </span>  
        <label><span>抽烟</span><input type="checkbox" v-model="userInfo.hobby" value="抽烟"/></label>  
        <label><span>喝酒</span><input type="checkbox" v-model="userInfo.hobby" value="喝酒"/></label>  
        <label><span>烫头</span><input type="checkbox" v-model="userInfo.hobby" value="烫头"/></label>  
        <br><br>  
        <span>所属校区: </span>  
        <label>  
            <select v-model="userInfo.city">  
                <option value="">请选择</option>  
                <option value="北京">北京</option>  
                <option value="上海">上海</option>  
                <option value="深圳">深圳</option>  
                <option value="湖北">湖北</option>  
            </select>  
        </label>  
        <br><br>  
        <label>  
            <span>其他</span>  
            <textarea v-model.lazy="userInfo.other"></textarea>  
        </label>  
        <br><br>  
        <label><input type="checkbox" v-model="userInfo.agree"/></label>  
        <span>阅读并接受<a href="">《用户协议》</a></span>  
        <br><br>  
        <button>提交</button>  
    </form>  
</div>  
  
<script>  
    Vue.config.productionTip = false;  
  
    const vm = new Vue({  
        el: "#app",  
        data: {  
            userInfo: {  
                account: '',  
                password: '',  
                sex: '',  
                hobby: [],  
                city: '',  
                other: '',  
                agree: false,  
                age: '',  
            }  
        },  
        methods: {  
            submit() {  
                // console.log(JSON.stringify(this._data))   不建议  
                console.log(JSON.stringify(this.userInfo))  
            }  
        }  
    });  
</script>  
</body>  
</html>
```


### Todo列表

组件间通过App.vue通信

**App.vue**

一般来说，不会将列表数据传入`Input`和`Foot`，为了可以在这两个组件实现添加和删除，可以在`App.vue`中定义添加删除的函数，只需要接收删除和添加的元素，在`App.vue`中完成，而子组件只需要传入函数，然后在事件函数中调用父组件传入的函数即可

如果想要数据本地存储，可以给 `TodoList` 添加侦听器（由于更改复选框属于更改数组中对象里的属性，所以需要深度侦听 `deep`），然后更改TodoList从浏览器本地获取，每次更新 `TodoList` 就将数据同步到浏览器

```vue
<template>  
  <div>  
	<Input :TodoList="TodoList"/>  <!-- 可以传入函数 addTodo -->
	<List :TodoList="TodoList"/>  
	<Foot :TodoList="TodoList"/>
  </div>  
</template>  
  
<script>  
import Input from "@/components/Input";  
import List from "@/components/List";  
import Foot from "@/components/Foot";  
  
export default {  
  name: "App",  
  data() {  
    return {  
      TodoList: [  
        {id: 0, msg: "起床", flag: false},  
        {id: 1, msg: "睡觉", flag: false},  
        {id: 2, msg: "吃饭", flag: false},  
      ]  
      // TodoList: JSON.parse(localStorage.getItem('TodoList')) || []
    }  
  },  
  components: {  
    Input,  
    List,  
    Foot  
  },
  //methods: {  
  //addTodo(obj) {  
  //  if (obj) {  
  //    this.TodoList.unshift(obj)  
  //  }  
  //},    
  // watch: {  
  //   TodoList: {  
  //     deep: true,  
  //     handler(value) {  
  //       localStorage.setItem("TodoList", JSON.stringify(value))  
  //     }  
  //   }  
  // }
}  
</script>  
  
<style>  
ul {  
  list-style: none;  
}  
</style>
```

**Input.vue**

id需要唯一标识，可以使用`Date.now()` 获取当前时间戳来作为id，除此之外，可以借助第三方库实现

* `uuid`：通过计算机地址物理地址国家等信息，生成全球唯一字符串标识，但体积很大
* `nanoid`：相当于 `uuid` 的简化版，体积非常小，直接调用 `nanoid()` 即可生成唯一标识

由于数据特殊，Vue无法侦听到对象中元素变化，所以可以采用 `v-model` 修改数据，但是不是很建议

```vue
<template>  
  <input type="txt" @keyup.enter="add" v-model.trim="msg"/> 
</template>  
  
<script>  
import {nanoid} from 'nanoid'

export default {  
  name: "Input",  
  data() {  
    return {  
      msg: ""  
    }  
  },  
  props: {  
    TodoList: {     // 可以不接受列表，改为函数 addTodo
      type: Array,  
      required: true  
    }  
  },  
  methods: {  
    add() {     // 在回调函数中处理好要添加的对象，然后调用 addTodo 即可
      if (this.msg) {  
        this.TodoList.unshift({id: nanoid(), msg: this.msg, flag: false})  
        this.msg = ""
      }  
    },  
  }  
}  
</script>  
  
<style scoped>  
  
</style>
```

**List.vue**

```vue
<template>  
  <div id="List">  
    <ul>  
      <li v-for="item in TodoList" :key="item.id">  
	    <!-- vue无法检测对象里数据的变化 -->
        <input type="checkbox" v-model="item.flag"/>  
        <span>{{item.msg}}</span>  
        <button @click="deleteList(item.id)">Delete</button>  
      </li>  
    </ul>  
  </div>  
</template>  
  
  
<script>  
export default {  
  name: "List",  
  props: {  
    TodoList: {  
      type: Array,  
      required: true  
    }  
  },  
  methods: {  
    deleteList(id) {  
      let index = this.TodoList.findIndex((item) => item.id === id)  
      this.TodoList.splice(index, 1)  
    }  
  }  
}  
</script>  
  
<style scoped>  
  
</style>
```

**Foot.vue**

```vue
<template>  
  <div>  
    <input type="checkbox" v-model="All">  
    <span>已完成{{ count }}/总{{ TodoList.length }}</span>  
    <button @click="clear">清除已完成</button>  
  </div>  
</template>  
  
<script>  
export default {  
  name: "Foot",  
  props: {  
    TodoList: {  
      type: Array,  
      required: true  
    }  
  },  
  methods: {  
    clear() {  
      let arr = this.TodoList.filter(item => !item.flag)  
      let n = this.TodoList.length  
      this.TodoList.splice(0, n)  
  
      arr.forEach(item => {  
        this.TodoList.push(item)  
      })  
    }  
  },  
  computed: {  
    count() {  
      let n = 0  
      this.TodoList.forEach((item) => {  
        n += item.flag ? 1 : 0  
      })  
      return n  
    },  
    All: {  
      get() {  
        if (this.TodoList) {  
          return false  
        }  
        return this.TodoList.every((item) => item.flag)  
      },  
      set(val) {  
        this.TodoList.forEach((item) => {  
          item.flag = val  
        })  
      }  
    }  
  },  
}  
</script>
```

**main.js**

```js
import Vue from 'vue'  
import App from '@/App.vue'  

Vue.config.productionTip = false  

new Vue({  
  render: h => h(App),  
	}).$mount('#app')
```

### Github 用户搜索

借助 api 通过输入框输入的关键词，来搜索与关键词相关的用户，并展示

api：`https://www.api.github.com/search/users?q=xxx`

返回数据详解

```json
{
"total_count": 105677,   // 总共有
  "incomplete_results": false,   // 是否显示所有
  "items": [
    {
      "login": "test",   // 用户名
      "id": 383316,      // 唯一标识 id
      "node_id": "MDQ6VXNlcjM4MzMxNg==",
      "avatar_url": "https://avatars.githubusercontent.com/u/383316?v=4",   // 头像地址
      "gravatar_id": "",
      "url": "https://api.github.com/users/test",   
      "html_url": "https://github.com/test",    // 主页
      "followers_url": "https://api.github.com/users/test/followers",
      "following_url": "https://api.github.com/users/test/following{/other_user}",
      "gists_url": "https://api.github.com/users/test/gists{/gist_id}",
      "starred_url": "https://api.github.com/users/test/starred{/owner}{/repo}",
      "subscriptions_url": "https://api.github.com/users/test/subscriptions",
      "organizations_url": "https://api.github.com/users/test/orgs",
      "repos_url": "https://api.github.com/users/test/repos",
      "events_url": "https://api.github.com/users/test/events{/privacy}",
      "received_events_url": "https://api.github.com/users/test/received_events",
      "type": "User",
      "site_admin": false,
      "score": 1.0
    }]
}    
```