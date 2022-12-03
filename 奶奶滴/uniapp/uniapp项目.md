## 项目结构

```
┌─uniCloud              云空间目录，阿里云为uniCloud-aliyun,腾讯云为uniCloud-tcb（详见uniCloud）
│─components            符合vue组件规范的uni-app组件目录
│  └─comp-a.vue         可复用的a组件
├─hybrid                App端存放本地html文件的目录，详见
├─platforms             存放各平台专用页面的目录，详见
├─pages                 业务页面文件存放的目录
│  ├─index
│  │  └─index.vue       index页面
│  └─list
│     └─list.vue        list页面
├─static                存放应用引用的本地静态资源（如图片、视频等）的目录，注意：静态资源只能存放于此
├─uni_modules           存放[uni_module](/uni_modules)规范的插件。
├─wxcomponents          存放小程序组件的目录，详见
├─main.js               Vue初始化入口文件
├─App.vue               应用配置，用来配置App全局样式以及监听 应用生命周期
├─manifest.json         配置应用名称、appid、logo、版本等打包信息，详见
├─pages.json            配置页面路由、导航条、选项卡等页面类信息，详见
└─uni.scss              这里是uni-app内置的常用样式变量 
```

## 发送请求

内部封装了`uni.request()`方法，在方法中传入配置对象，配置对象中除了正常的`url`和`method`还有成功的回调和失败的回调，具体参考[uniapp开发指南](https://uniapp.dcloud.net.cn/api/request/request.html)

例如

```js
uni.request({
	url: "http://www.api.droliz.cn/api/other/juzi",
	method: 'GET',
	success(res) {
		console.log(res.data)
	},
	fail(err) {
		console.log(err)
	}
})
```

## 组件间通信

### 子传父

通过监听自定义事件`this.$emit('事件', 参数)`

### 父传子

通过`props`传入

### 兄弟组件

需要借助`uni.$emit`、`uni.$on`、`uni.$off`、`uni.$once`，这四者常用于跨页面、跨组件通讯

接收组件：通过 `uni.$on(eventName,callback)` 监听全局的自定义事件，事件可以由 `uni.$emit` 触发，回调函数会接收所有传入事件触发函数的额外参数

发送组件：通过 `uni.$emit(eventName,OBJECT)` 触发全局的自定事件。附加参数都会传给监听器回调

例如

接收组件

```vue
<template>
	<view>
		comp2，接收的消息：{{msg}}
	</view>
</template>

<script>
	export default {
		name: "comp2",
		data() {
			return {
				msg: ""
			};
		},
		created() {
			uni.$on('getMsg', (msg) => {   // 监听自定义事件
				this.msg = msg
			})
		},
		deactivated() {
			uni.$off('getMsg')
		}
	}
</script>
```

发送组件

```vue
<template>
	<view>
		<button type="primary" @click="sendMsg">发送消息</button>
	</view>
</template>

<script>
	export default {
		name: "comp",
		data() {
			return {
				msg: "消息"
			};
		},
		methods: {
			sendMsg() {
				uni.$emit('getMsg', this.msg)   // 自定义事件 getMsg
			}
		}
	}
</script>
```

### 全局数据共享

#### 通过Vue原型链

在 Vue 的原型上添加需要全局共享的数据

```js
Vue.prototype.globalText = "Vue原型上的全局数据"

// 获取数据
this.globalText
```

#### 通过globalData共享数据

在 `App.vue` 中定义

```js
export default {
	globalData: {
		msg: "全局消息",
		baseUrl: "公共的请求url"
	}
}
```

获取数据

```js
getApp().globalData.msg
```

修改数据

```js
getApp().globalData.msg = "修改数据"
```

**但使用的时候要注意，在onLaunch，onShow生命周期函数中不能直接使用`getApp().globalData，会报错`**

## 字体图标



