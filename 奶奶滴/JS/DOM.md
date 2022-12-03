## DOM

[document对象手册](https://www.runoob.com/jsref/dom-obj-document.html)

* DOM（文档对象模型），是处理HTML或者XML的程序接口
* DOM树
![DOM树](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimage.morecoder.com%2Farticle%2F201810%2F20181024121100924078.gif&refer=http%3A%2F%2Fimage.morecoder.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1645775144&t=045d90ff45b71447a5e81c606f7b24f5)
    * 文档：一个页面就是一个文档，DOM中用document表示
    * 元素：页面中的所有标签都是元素，DOM中用element表示
    * 节点：网页的所有内容都是节点，DOM中用node表示
    >DOM将以上内容都看作对象

---
### 获取页面元素

DOM在实际开发中主要用于操作元素

#### 根据ID获取
* 使用getElementById('id')方法获取元素，返回一个元素对象

```html
<body>
	<div id="time">2022-1-26</div>
	<script>
		var timer = document.getElementById('time');
		// 打印返回元素对象
		console.dir(timer);
	</script>
</body>
```

#### 根据标签名获取
* 使用getElementsByTagName('标签名')方法获取带有标签名的对象集合，以伪数组的形式返回

```html   
<body>
	<ul>
		<li></li>
		<li></li>
		<li></li>
		<li></li>
		<li></li>
	</ul>
	<script>
		var lis = document.getElementsByTagName('li');
		// 指定: 父元素.getElementsByTagName(子元素);
		// 指定的父元素必须是单个对象
	</script>
</body>
```

* **通过HTML5新增的方法获取（不支持i9以下）**
    * 使用getElementsByClass('类名')方法获取

```html
<body>
	<div class="time">2022-1-26</div>
	<script>
		var timer = document.getElementByClass('time');
	</script>
</body>
```

* querySelector('选择器')方法返回指定选择器的元素集合第一个元素
* querySelectorAll('选择器')方法返回指定选择器的所有元素

---
### 事件

#### 事件组成

* 事件源：事件被触发的对象
* 事件类型：事件被触发的类型[事件类型参考](https://www.runoob.com/jsref/dom-obj-event.html)
* 事件处理程序：通过一个函数赋值的方式完成

示例:    

```html
<body>
	<button id="btn">按钮</button>
	<script>
		var btn = document.getElementById('btn');
		// onclick：鼠标点击
		btn.onclick = function() {alert('按钮！');}
	</script>
</body>
```

#### 执行事件的步骤

* 获取事件源
* 注册事件（绑定事件）
* 添加事件处理程序（通过函数赋值方式）

---

### 操作元素

#### 改变元素内容
* innerText、innerHTML

示例:

```html
<body>
	<button>显示当前时间</button>
	<div>某个时间是:</div>
	<script>
		var btn = document.querySelector('button');
		var div = document.querySelector('div');
		btn.onclick = function() {
			var date = new Date();
			var year = date.getFullYear();
			var month = date.getMonth() + 1;
			var dates = date.getDate();
			var arr = [ '星期日', 
						'星期一',
						'星期二',
						'星期三',
						'星期四',
						'星期五',
						'星期六'
					]
			var day = date.getDay();
			div.innerHTML = year + '-' + month + '-' + dates + '-' + arr[day];
		}
	</script>
</body>
```

>innerHTML识别html语句，innerText不识别html语句而且会去除换行（会直接输出）
>
>都是可读写的，可以获取元素里面的内容

#### 修改元素属性

示例：(点击按钮更改图片)

```html
<body>
	<button id="zxy"></button>
	<button id="ldh"></button>
	<img src="#" alt="" title="">
	<script>
		var zxy = document.getElementById('zxy');
		var ldh = document.getElementById('ldh');
		zxy.onclick = function() {
			img.src = 'http://';
			img.title = '张学友';
		}
		ldh.onclick = function() {
			img.src = 'http://';
			img.title = '刘德华';
		}
	</script>
</body>    
```
    
#### 表单元素属性的操作

示例:（登录密码显示和隐藏）

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>登录界面</title>
	<style>
		* {
			margin: 0;
			padding: 0;
		}

		.box {
			width: 300px;
			height: 600px;
			border-radius: 50px;
			border-color: #111;
			margin: 100px auto;
		}
		
		input {
			
			color: #808080;
			width: 200px;
			margin: 0 auto;
			outline: none;
			border: 0;
			border-bottom: 1px solid #ccc;
		}

		button {
			float: right;
			border: 0;
		}

	</style>
</head>
<body>
	<div class="box">
		<div>
			<input type="text" placeholder="请输入账号">
		</div>
		<div>
			<input type="password" placeholder="请输入密码" id="pwd">
			<button id="eye">显示密码</button>
		</div>
	</div>
	<script>
		var input_password = document.getElementById("pwd");
		var btn = document.getElementById("eye");
		var flag = 0;
		btn.onclick = function() {
			if (flag == 0) {
				input_password.type = 'text';  
				flag = 1;  
			}
			else {
				input_password.type = 'password';
				flag = 0;
			}   
		}
	</script>
</body>
</html>
```

#### 修改样式属性

```js
// 样式比较少时使用
// 属性名是采用的驼峰命名法，不同于CSS的background-color
element.style.backgroundColor: "red";

// 样式较多时使用类来更改(会覆盖之前的类)
element.className = "one";
```

**排他思想**

* 如果同一组元素，想要实现某一个元素相同的样式：
	* 先将所有元素样式清空
	* 再给当前元素添加样式

示例:(按钮切换)

```html
<body>
<button>按钮1</button>
<button>按钮2</button>
<button>按钮3</button>
<button>按钮4</button>
<button>按钮5</button>
<script>
	var btns = document.getElementsByTagName('button');

	for (var i = 0; i < btns.length; i++) {
		btns[i].onclick = function() {
			// 先清除所有的背景，以达到点击按钮时，其他按钮全部没有背景
			for (var j = 0; j < btns.length; j++) {
				btns[j].style.backgroundColor = '';
			}
			// 让当前按钮改为pink
			this.style.backgroundColor = 'pink';
		}
	}
</script>
</body>
```

示例: （表单全选/取消全选）

```html
<div class="wrap">
	<table>
		<thead>
			<tr>
				<th>
					<input type="checkbox" id="j_cbAll">
				</th>
				<th>商品</th>
				<th>价钱</th>
			</tr>
		</thead>
		<tbody id="j_tb">
			<tr>
				<td>
					<input type="checkbox">
				</td>
				<td>iPhone8</td>
				<td>8000</td>
			</tr>
			<tr>
				<td>
					<input type="checkbox">
				</td>
				<td>iPhone8</td>
				<td>8000</td>
			</tr>
			<tr>
				<td>
					<input type="checkbox">
				</td>
				<td>iPhone8</td>
				<td>8000</td>
			</tr>
			<tr>
				<td>
					<input type="checkbox">
				</td>
				<td>iPhone8</td>
				<td>8000</td>
			</tr>
			<tr>
				<td>
					<input type="checkbox">
				</td>
				<td>iPhone8</td>
				<td>8000</td>
			</tr>
		</tbody>
	</table>
</div>
<script>
	// 全选按钮
	var j_cbAll = document.getElementById('j_cbAll');
	// 单个按钮
	var j_tb = document.getElementById('j_tb').getElementsByTagName('input');
	j_cbAll.onclick = function() {
		for (var i = 0; i < j_tb.length; i++) {
			j_tb[i].checked = this.checked;
		}
	}
	 // 如果下面的按钮全部选择时，全选按钮自动勾上
	for (var i = 0; i < j_tb.length; i++) {
		j_tb[i].onclick = function() {
			var flag = true;
			// 每次点击，检查所有的是否都是选择
			for (var j = 0; j < j_tb.length; j++) {
				if (!j_tb[j].checked) {
					flag = false;
					break;
				}
			}
			j_cbAll.checked = flag;
		}
	}
</script>
```

#### 自定义属性的操作

目的：为了保存并使用数据，有些数据可以保存再页面中而不用保存在数据库中，规定以data开头

获取属性值
* `element.属性`获取内置属性值
* `element.getAttribute('属性')`主要获取自定义属性值

设置属性值
* `element.属性 = '值'`设置内置属性值
* `element.setAttribute('属性', '值')`主要设置自定义属性值

移除属性
* `element.removeAttribute('属性')`

示例（tab切换栏）

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>页面</title>
	<style>
		* {
			margin: 0;
			padding: 0;
			box-sizing: border-box;    
		}

		.table {
			width: 800px;
			background-color: #fff;
			margin: 100px auto;
		}

		.current {
			background-color: red;
			color: #fff;
		}

		.tab_list {
			border: 1px solid gray;
			height: 39px;
			background-color: rgb(238, 222, 222);
			margin-top: 10px;
		}

		.tab_con {
			margin-top: 10px;
			height: 100px;
		}

		ul {
			list-style: none;
		}

		.tab_list li {
			float: left;
			line-height: 39px;
			padding: 0 20px;
			text-align: center;
			cursor: pointer;
		}

		.item_info {
			padding: 20px 0 0 20px;
		}

		.item {
			display: none;
		}

	</style>
</head>
<body>
	<div class="table">
		<div class="tab_list">
			<ul>
				<li class="current">商品介绍</li>
				<li>规格与包装</li>
				<li>售后保障</li>
				<li>商品评价</li>
				<li>手机社区</li>
			</ul>
		</div>
		<div class="tab_con">
			<div class="item" style="display: block;">
				商品介绍内容
			</div>
			<div class="item">
				规格与包装内容
			</div>
			<div class="item">
				售后保障内容
			</div>
			<div class="item">
				商品评价内容
			</div>
			<div class="item">
				手机社区内容
			</div>
		</div>
	</div>

	<script>
		var lis = document.querySelector('.tab_list').querySelectorAll('li');

		var items = document.querySelectorAll('.item');

		for (var i = 0; i < lis.length; i++) {
			lis[i].setAttribute('date-index', i);

			lis[i].onclick = function() {
				for (var j = 0; j < lis.length; j++) {
					lis[j].className = '';
				}
				this.className = 'current';
				var index = this.getAttribute('date-index');

				for (var j = 0; j < items.length; j++) {
					items[j].style.display = 'none';
				}
				items[index].style.display = 'block';
			}          
		}
	</script>
</body>
</html>
```

---
### 节点操作

#### 获取元素的两种方式

* 利用DOM的方法获取元素，逻辑性不强、繁琐（元素操作）
* 利用节点层级关系获取元素 逻辑性强、兼容性差 （节点操作）

>相比较之下，节点操作更为简单

#### 节点描述

网页中的所有内容（包括空格换行）都是节点，用node表示.HTML DOM树中的所有节点均可通过JS进行访问，都可以被创建或删除

一般的节点都有nodeType、nodeName、nodeValue三个基本属性
* 元素节点：nodeType 为 1
* 属性节点：nodeType 为 2
* 文本节点：nodeType 为 3（包括空格、换行、文字）

#### 节点层级

利用DOM树可以将节点划分为不同的层级关系

**父节点**

```js
// 获取子节点
var son = document.getElementById('son');
// 离son最近的父节点，如果没有，返回空
var parent = son.parentNode;
```

**子节点**

* 标准

```js
var parent = document.getElementById('parent');
// 所有的子节点，包括空格和换行
var son = parent.childNodes;
// 可以利用nodeType来筛选想要的元素
```

* 非标准

```js
var parent = document.getElementById('parent');
// 获取所有子元素节点
var son = parent.children;
```

firstChild、lastChild分别时获取第一个和最后一个子节点（所有的子节点中选取）

firstElementChild、lastElementChild分别是获取第一个和最后一个子元素节点（子元素节点中选取 IE9一下不兼容）

* 兄弟节点
* node.nextSibling 获取node的下一个 ***节点***（包括空格、换行）未找到返回Null
* node.previousSibling 获取node的上一个节点
* node.nextElementSibling 获取下一个 ***元素节点***
* node,previousElementSibling 获取上一个 ***元素节点***

封装一个兼容性函数，解决兼容性问题

```js
function getNextElementSibling(element) {
	var el = element;
	while (el = el.nextSibling) {
		if (el.nodeType === 1) {
			return el;
		}
	}
	return null;
}
```

**创建节点、添加节点**

* `document.createElement('元素')` 创建元素节点
* `node.appendChild(child)` 在末尾添加节点
* `node.insertBefore(child, 元素)`在前面添加元素

>创建的是一个孩子元素节点，应用于评论区等

**删除节点**

* `node.removeChild(元素)`删除指定子元素节点

**复制节点**

* `node.cloneNode(布尔值)`克隆指定元素节点，默认为false，为浅拷贝，只复制标签.改为true为深拷贝，复制标签和内容

**三种创建元素的区别**

* `document.write()`
* 直接写入页面的内容，但是文档流执行完毕，则会导致页面全部重绘
* `element.innerHTML`
* 采用字符串拼接的形式，数据多时，效率低下
* 但如果采用数组形式，效率比createElement()高

```js
// 采用字符串拼接形式
var inner = document.querySelector(选择器);
for (var i = 0; i < 次数; i++) {
	inner.innerHTML += 元素代码
}

// 采用数组的形式添加
var arr = [];
for (var i = 0; i < 个数; i++) {
	arr.push(元素代码);
}
// document.父元素.innerHTML = arr.join('');
```

* `document.createElement()`
* 效率高，好用

### DOM重点核心

* 关于DOM的操作，主要针对元素的操作，主要有创建、增、删、改、查、属性操作、事件操作

#### 事件高级

**元素注册事件的两种方式**

* `element.事件 = function() {}`  

>传统方法on开头的，具有唯一性,同一元素只能设置一个函数(后面的会覆盖前面的)

* `addEventListener(type, listener[, useCapture])`    

>利用方法监听事件，同一元素同一事件可以注册多个监听事件

* type: 事件类型字符串如：click（不带on）
* listener：事件处理函数（不需要调用，只用写函数名）
* useCapture：可选参数，默认false，与DOM事件流有关

**删除事件的两种方式**
* `eventTarget.事件 = null;`

>传统的解绑事件方式

* `eventTarget.removeEventListener(type, listener[, useCapture])`

>解绑事件监听

#### DOM事件流

事件发生时会在元素节点之间按照特定的顺序传播，这个传播过程就是DOM事件流

![DOM事件流](http://www.droliz.cn/markdown_img/DOM事件流.jpg)

DOM事件流的三个阶段：捕获阶段 -> 当前目标阶段 -> 冒泡阶段

传统的事件添加是在冒泡阶段调用事件处理程序，如果添加事件监听的useCapture的值是true，表示事件在捕获阶段就调用事件处理程序，否则就是在冒泡阶段调用事件处理程序

#### 事件对象

对于不同的事件，事件对象（event简写e）有不同的属性和方法，写在function（事件对象）中，不用传入实参，系统自动创建

| 属性和方法          | 说明                           |
| :------------------ | :----------------------------- |
| e.target            | 返回出发事件的对象             |
| e.type              | 返回事件类型，不带on           |
| e.preventDefault()  | 阻止默认事件，如：不让链接跳转 |
| e.stopPropagation() | 阻止冒泡                       |

[更多](https://www.runoob.com/jsref/dom-obj-event.html)      

**阻止冒泡**
* 利用事件对象的`stopPropagation()`方法

**事件委托**

* 事件委托也叫事件代理，在jQuery里面称为事件委派
* 给父节点设置事件监听，然后利用冒泡原理来邮箱设置的每个子节点提高性能