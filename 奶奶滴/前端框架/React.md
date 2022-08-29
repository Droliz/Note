# React

## 了解 react 

### React 概述

#### 简述

React 是一个用于构建用户界面的 JS 库，主要用于写 HTML 页面，或构建 Web 应用

从 MVC 角度看，React 只是视图层 V ，并没有提供完整的 M 和 C 的功能

#### 特点

* 声明式
	只需要描述 UI 看起来的样子，与 HTML 一样，React 负责渲染 UI，并在数据变化时更新 UI
* 基于组件
* 学习一次，随处可用
	开发 Web 应用
	开发移动端原生应用（react-native）
	开发 VR 应用（react360）

### React 的基本使用

#### 安装 React

安装 React 的核心包 `react`，提供创建元素、组件等功能

```sh
npm i react
```

安装 DOM 相关功能的包 `react-dom`

```sh
npm i react-dom
```

#### 简单使用 React

1、在页面中先后引入 react 和 react-dom

```html
<!-- index.html -->
<script src="./node_modules/react/umd/react.development.js"></script>  
<script src="./node_modules/react-dom/umd/react-dom.development.js"></script>
```

2、创建 React 元素，并渲染

```html
<div id="root"><div>
<script>
    // createElement('元素标签', ’属性‘, '元素子节点1', '元素子节点2' ...)   
    // 从第三个开始全是子节点，可以嵌套创建
    // 属性使用对象的形式 {title: '标题', id: '1'}
    const title = React.createElement('h1', null, 'Hello React')  
    // render(要渲染的元素, DOM 对象)  
    ReactDOM.render(title, document.getElementById('root'))  
</script>
```


### React 脚手架的使用

#### 初始化项目

使用 npx 命令初始化项目，npx 是npm v5.2.0 引入，如果不使用npx命令，则需要先全局安装脚手架，然后再使用初始化

初始化成功会显示 `happy hacking`

```sh
npx create-react-app project_name
```

#### 启动项目

项目启动成功，自动打开浏览器

```sh
# 项目根目录
npm start

or 

yarn start
```

## JSX

### JSX 简介

什么是JSX：`JSX = javascript xml` 就是Javascript和XML结合的一种格式。是 JavaScript 的语法扩展，**只要你把HTML代码写在JS里，那就是JSX**

JSX 执行更快，因为它在编译为 JavaScript 代码后进行了优化。(通过 `@babel/preset-react` 编译 JSX 语法)

它是类型安全的，在编译过程中就能发现错误。

使用 JSX 编写模板更加简单快速。

相比 `createElement()` 方法渲染，JSX 更加清晰页面结构、直观、简洁

```js
// 使用 createElement 方法
React.createElement(
	'div',
	{className: 'shopping-list'},
	React.createElement('h1', null, 'Shopping List'),
	React.createElement(
	'ul',
	null,
	React.createElement('li', null, 'Instagram'),
	React.createElement('li', null, 'WhatsApp')
	)
)
```


```jsx
// 使用 JSX
<div className='shopping-list'>
	<h1>Shopping List</h1>
	<ul>
		<li>Instagram</li>
		<li>WhatsApp</li>
	</ul> 
</div>
```

### JSX 的基本使用

#### 创建 react 元素

使用 JSX 语法创建 react 元素

```jsx
const title = <h1>标题</h1>

// 小括号包裹
const title = (
	<h1>标题</h1>
)
```

#### 渲染元素

使用 `render` 方法渲染元素

```jsx
ReactDOM.render(title, root)
```

#### 注意

JSX 渲染元素的属性名采用驼峰命名法

JSX 渲染元素添加属性时，有对应的特殊属性名
* class -> className
* or -> htmlFor
* abindex -> tabIndex

如果 react 没有子节点，使用 `/>` 结束，例如 `<span />`

一般使用小括号包裹 JSX，从而避免 JS 中的自动插入分号

### JSX 中使用 JS 表达式

#### 嵌入式 JS 表达式

类似于 vue 的插值表达式，区别为单双括号
```jsx
const name = 'Jack'
const dv = (
	<p>name = {name}</p>
)

const func = () => {
	'good'
}
const dv = (
	// 函数调用表达式
	<p>{func()}</p>
)
```
需要注意的是，JSX 本身也是一个 JS 表达式，所以在嵌入式表达式中可以写 JSX 代码

### JSX 的条件渲染

根据条件渲染特定的 JSX 结构（三元表达式或者逻辑运算）

场景：页面跳转 loading 效果

```jsx
// App.js
const flag = true
  
function App() {
  return (
    <div className='App'>
      {flag ? (<span>name</span>) : null}
    </div>
  )
}
```


### JSX 的列表渲染

实现页面中重复结构的渲染，在 vue 中是通过 `v-for` 指令实现的，而在 react 中是通过数组提供的 `map` 方法实现的

遍历列表需要提供一个类型为 number/string 的不可重复的 key 提高 diff 性能。key只会在内部使用，不会出现在真实的 DOM 结构中

```jsx
// App.js
const songs = [
  { id: 1, name: '像我这样的人' },
  { id: 2, name: '七月上' },
  { id: 3, name: '荷塘月色' }
]
function App() {
  return (
    <div className="App">
      <ul>
        {songs.map(song => <li  key={song.id}> {song.name} </li>)}
      </ul>
    </div>
  );
}
export default App;
```


#### 注意

为了使代码模板简洁，在渲染 DOM 元素时，如果遇到复杂的逻辑，可以先封装为一个方法，在真正渲染时，只调用方法

```jsx
// App.js
const getDOM = (type) => {
  if (type === 1) {
    return <h1>type = 1</h1>
  } else if (type === 2) {
    return <h1>type = 2</h1>
  } else if (type === 3) {
    return <h1>type = 3</h1>
  }
}
  
function App() {
  return (
    <div className='App'>
      {getDOM(1)}
      {getDOM(2)}
      {getDOM(3)}
    </div>
  )
}
```

### JSX 的样式处理

#### 行内样式 

在 DOM 元素上绑定 `style` 属性，使用对象的形式，将属性参数以键值对的形式传入

```jsx
// 属性采用驼峰命名法
const styles = {
  color: 'pink',
  backgroundColor: 'black'
}
  
function App() {
  return (
    <div className='App'>
      <span style = {styles}>this is span</span>
    </div>
  )
}
```

#### 类名样式

在 DOM 元素上绑定 `className` 属性，通过导入样式

```jsx
// 导入样式
import './App.css'

function App() {
  return (
    <div className='App'></div>
  )
}
```

#### 动态类名控制

在类名样式的基础上，通过三元表达式，实现动态控制类名

```jsx
import './App.css'

const flag = true

function App() {
  return (
    <div className={flag ? App : ''}></div>
  )
}
```

### 注意

* JSX 中必须有一个根节点，如果不想有，可以使用幽灵节点 `<></>` 代替
* JSX 中的标签都必须闭合，`/>`



## 案例

### 评论区案例

#### 