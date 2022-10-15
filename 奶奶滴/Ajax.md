# Ajax (数据交互&异步编程) and Git

## 基础

***客户端与服务器***
* 服务器：上网过程中，**负责存放和对外提供资源**的电脑
* 客户端：上网过程中，**复杂获取和消费资源**的电脑

***URL***
* URL：统一资源定位符，用于表示互联网上每个资源的唯一存放位置，浏览器只有通过URL才能确定定位资源位置
* URL组成
	* 客户端与服务器之间的**通信协议**
	* 存有该资源的**服务器名称**
	* 资源在服务器上的**具体存放位置**
        ![](http://www.droliz.cn/markdown_img/URL组成.jpg)

**网页的打开过程***
* 客户端与服务器之间的通信过程分为三个步骤：**请求-处理-响应**

***服务器对外提供的资源***
* 网页中的所有数据都是服务器对外提供的资源
* 如果要在网页中请求服务器上的数据资源，则需要用到`XMLHttpRequest`对象，是浏览器提供的js成员
* `var xhrObj = new XMLHttpRequest();`

资源请求的方式
* 最常见的请求方式分别为 get 和 post
	* get请求常用于**获取服务器资源**
	* post请求常用于**向服务器提交数据**

### Ajax

#### 简介

Ajax：Asynchronous JavaScript And XML（异步JavaScript 和XML），在网页中利用XMLHttpRequest对象和服务器进行数据交互的方式

Ajax能轻松实现网页与服务器之间的数据交互（**无刷新获取数据但没有浏览历史，存在跨域问题**）

场景：
* 注册用户时，动态的检测用户名是否被占用
* 搜索关键字时，动态加载搜索提示列表
* 数据分页显示时，点击翻页根据页码动态刷新表格数据
* 数据的增删改查

**XML***
* XML 可扩展标记语言，用来传输和存储数据（**现已被JSON取代**）
* XML和HTML类似，不过XML中没有预定义标签，全都是自定义标签，用来表示一些数据

示例

```xml
有一个学生数据：name='张三';age='20';gender='男'
XML
<student>
	<name>张三</name>
	<age>20</age>
	<gender>男</gender>
</student>
```

#### jQuery中的Ajax

jQuery对XMLHttpRequest进行了封装，提供一系列相关函数，极大的**降低了Ajax的使用难度**

Jquery中发起Ajax请求：
* `$.get()`
	* `$.get(url[, data, callback])`获取资源
	* url：要请求的资源地址
	* data：请求资源期间要携带的参数
	* callback：请求成功的回调函数

```html
// 图书管理
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>图书管理</title>
	<link rel="stylesheet" href="http://www.droliz.cn/css/lib/bootstrap-3.4.1-dist/bootstrap.css">

	<script src="http://www.droliz.cn/JavaScript/lib/jQuery.js"></script>

	<script>
		$(function () {
			// 使用get请求向服务器获取数据
			function getBookList() {
				$.get('http://www.liulongbin.top:3006/api/getbooks', function (response) {
					// console.log(response)
					// 判断是否获取到数据
					if (response.status !== 200) {
						return alert('获取数据失败');
					}

					// 渲染页面结构
					var rows = [];
					// 使用循环拼接每一行
					$.each(response.data, function (i, item) {
						rows.push(
							'<tr><td>' + item.id +
							'</td><td>' + item.bookname +
							'</td><td>' + item.author +
							'</td><td>' + item.publisher +
							'</td><td>' +
							'<a href="javascript:" class="del" data-id="' + item.id + '">删除</a></td></tr>'
						);
					});
					// 拼接完成后添加到页面
					$('tbody').html(rows.join(''));
				});
			}

			// 初始化页面
			getBookList();

			// 给每一行的删除按钮绑定事件(后期被添加的元素必须用事件的委派)
			$('tbody').on('click', '.del', function () {
				// 根据自定义属性获取到id
				var id = $(this).attr('data-id');
				// 发送get请求删除数据(根据id删除数据)
				$.get('http://www.liulongbin.top:3006/api/delbook', { id: id }, function (response) {

					// 判断是否删除成功
					if (response.status !== 200) {
						return alert('删除失败');
					}
					// 删除成功后重新获取数据
					getBookList();
				});
			});

			// 给添加按钮绑定事件
			$('#btnAdd').on('click', function () {
				// 获取用户输入的数据trim去除两端的空格
				var bookname = $('#iptBookname').val().trim();
				var author = $('#iptAuthor').val().trim();
				var publisher = $('#iptPublisher').val().trim();
				// 判断用户是否输入了数据
				if (!bookname || !author || !publisher) {
					return alert('请输入完整的数据');
				}
				// 发送post请求添加数据
				$.post('http://www.liulongbin.top:3006/api/addbook', {
					bookname: bookname,
					author: author,
					publisher: publisher
				}, function (response) {
					// 判断是否添加成功
					if (response.status !== 201) {
						return alert('添加失败');
					}
					// 添加成功后重新获取数据
					getBookList();
					// 清空输入框
					$('#iptBookname').val('');
					$('#iptAuthor').val('');
					$('#iptPublisher').val('');
				});
			});
		});
	</script>

</head>

<body>
	<!-- 添加图书的panel面板 -->
	<div class="panel panel-primary">
		<div class="panel-heading">
			<h3 class="panel-title">图书管理</h3>
		</div>

		<!-- 搜索栏 -->
		<div class="panel-body form-inline">
			<!-- form-inline类可以将搜索栏换为同排 -->
			<div class="input-group">
				<div class="input-group-addon">书名</div>
				<input type="text" class="form-control" id="iptBookname" placeholder="请输入书名"/>
			</div>

			<div class="input-group">
				<div class="input-group-addon">作者</div>
				<input type="text" class="form-control" id="iptAuthor" placeholder="请输入作者"/>
			</div>

			<div class="input-group">
				<div class="input-group-addon">出版社</div>
				<input type="text" class="form-control" id="iptPublisher" placeholder="请输入出版社"/>
			</div>
			<!-- 按钮 -->
			<button type="button" class="btn btn-primary" id="btnAdd">添加</button>
		</div>

		<!-- 列表 -->
		<table class="table table-hover table-bordered">
			<thead>
				<tr>
					<th>id</th>
					<th>书名</th>
					<th>作者</th>
					<th>出版社</th>
					<th>操作</th>
				</tr>
			</thead>
			<tbody id="tbody">
				<!-- 动态添加表格 -->
			</tbody>
		</table>
	</div>

</body>

</html>
```

##### form表单

表单再网页中主要负责**数据采集功能**，HTML中的form标签，就是用于采集用户输入的信息，并通过form标签提交操作，将采集到的信息提交到服务器端进行处理

表单由三个基本部分组成的：表单标签、表单域、表单按钮
* 表单域：文本框、密码框、复选框等（用来采集用户输入的信息）

表单的属性，规定如何把采集来的数据发送到服务器（**表单如果想要提交，表单域必须要有name属性**）

| 属性    | 值  | 描述 |
| :-- | :--- | :-- |
| action  | URL地址| 规定但提交表单时，向何处发送表单数据|
| method  | get或post | 规定以何种方式把表单数据提交到action URL |
| enctype | application/x-www-form-urlencoded、multipart/form-data、text/plain | 发送表单数据之前如何加密                 |
| target  | \_blank、\_self、\_parent、\_top、framename | 规定再何处打开action URL|

* action
	* 这个URL由后端提供，专门负责接收表单提交过来的数据，默认URL为当前页面的URL（当提交表单后，页面会立即跳转到指定的URL）
* method
	* get适合提交少量的、简单的数据，post适合提交大量的、复杂的、或包含文件上传的数据
* enctype（一般不涉及文件上传，就使用默认值）
	* `application/x-www-form-urlencoded`在发送前编码所有字符（默认）
	* `multipart/form-data`不对字符编码，**在表单包含文件上传时，必须使用该值**
	* `text/plain`空格转换为"+"加号，但不对特殊字符编码
* target
	* 默认值为_self，在当前框架打开URL，最常用的是_blank在新窗口中打开

#### 表单同步提交

通过点击 submit 按钮触发提交事件，从而跳转到指定的URL的行为，叫做表单的同步提交

缺点
* `<form>`表单提交后，整个页面会发生跳转，用户体验极差
* `<form>`表单提交后，页面之前的状态和数据会丢失

>可以通过让Ajax负责提交数据到服务器，表单只负责采集数据来解决此问题

**通过Ajax来提交表单数据**

* 1、可以对提交事件添加事件监听
* 2、阻止表单默认的提交行为
* 3、快速获取表单中的数据
	* `serialize()`函数(可以一次性获得表单中所有的数据)
	* 获取到的数据格式：`name=xxx&content=xxx&id=xxx`每组数据以&连接


```js
$('form').on('submit', function(e) {
	// 阻止表单的提交和页面的跳转
	e.preventDefault();
});
```


### 模板引擎

在UI渲染时，由于字符串的拼接进行的添加内容，但UI结构复杂时，字符串拼接很复杂，修改起来也是很麻烦的，所以**模板引擎：根据指定的模板结构和数据 ，自动生成一个完整的HTML页面**

* 要先指定模板结构和数据，提交给模板引擎，生成一个完整的HTML
* 减少了字符串的拼接
* 使代码结构更清晰，利于阅读和后期维护

**art-template模板引擎**

[art-template](http://aui.github.io/art-template/zh-cn/docs/installation.html)
* 将页面中`art-template.js`文件下载

**模板引擎使用步骤**

* 导入art-template
* 定义数据
* 定义模板 ({{}}双花括号代表占位，用于替换数据) 
* 调用`template(模板id, 数据)`函数
* 渲染HTML

```html
<!-- 模板引擎 -->

<div id="content"></div>
<!-- 模板 -->
<script type="text/html" id="tpl-user">   <!-- 将这个标签中的内容当作html解析 -->
	<!-- {{}}双花括号代表占位 -->

	<div id="#title">员工信息</div>
	<div>姓名:<span id="name">{{name}}</span></div>
	<div>年龄:<span id="age"></span>{{age}}</div>
	<div>会员:<span id="isVIP"></span>{{isVIP}}</div>
	<div>注册时间:<span id="regTime"></span>{{regTime}}</div>
	<div>爱好
		<ul id="hobby">
			<!-- 循环 -->
			{{each hobby}}
			<li>{{$value}}</li>
			{{/each}}
		</ul>
	</div>
</script>

<script>
	// 数据
	var data = {
		name: '张三',
		age: 18,
		isVIP: true,
		regTime: new Date(),
		hobby: ['篮球', '足球', '排球']
	}

	var html = template('tpl-user', data);
	$('#content').html(html);
</script>
```

**模板引擎语法**

* {{}}双花括号不仅有**变量输出**的作用，还有**循环数组**等作用

输出

```js
{{value}}
{{obj.key}}
{{obj['key']}}
{{a ? b : c}}
{{a || b}}
{{a + b}}
```

原文输出：`{{@ value}}`如果value中包含了html标签，可以加一个@保证html被正常的渲染

条件输出：`{{if v1}} 内容1 {{else if v2}} 内容2 {{/if}}`

循环输出：`{{each 循环数组}}内容{{/each}}`可以通过`{{$index}}和{{$value}}`获取当前的索引和值

* 过滤器
![过滤器](http://www.droliz.cn/markdown_img/过滤器.jpg) 

`{{value | filterName}}`将Value当做参数传入后面的函数，返回过滤后的新值（类似管道操作符）

**定义过滤器的基本语法**

`template.defaults.imports.filterName = function (value) {// return 处理结果}`

```js
// regTime = new Data();
// 时间过滤器
<div> 当前的时间: {{regTime | getTime}} </div>

template.defaults.imports.getTime = function (data) {
	var y = data.getFullYear();
	var m = data.getMonth() + 1;
	var d = data.getDate();
	// 必须有返回值
	return y + '-' + m + '-' + d
} 
```
* **模板引擎实现原理**
* 正则与字符串操作

[正则表达式语法](https://www.runoob.com/regexp/regexp-syntax.html)

* `exec()`函数用于**检索字符串**中的正则表达式的匹配，如果有，返回匹配值，负责返回null`RegExpObject.exec(string)`

```js
// 匹配字符串
var str = 'hello'
// 匹配规则用//包裹起来
var pattern = /o/   
console.log(pattern.exec(str))
// 结果:["o", index: 4, input: 'hello', groups: undefined] 
```

分组:用()包裹起来的内容表示一个分组，可以通过分组提取自己想要的内容`/{{([a-zA-Z]+)}}/`将[a-zA-Z]+作为一个分组

```js
var s = "<div>我是{{name}}</div>";
// 匹配 {{name}}
var reg = /{{([a-zA-Z]+)}}/
var pattern = reg.exec(s)
console.log(pattern);
// 结果：['{{name}}', 'name', index: 7, '<div>我是{{name}}</div>', groups: undefined]
``` 

字符串的`replace(旧字符, 新字符)`函数

用一些字符替换另一些字符

## 起灰

### XMLHttpRequest的基本使用

![XMLHttpRequest](http://www.droliz.cn/markdown_img/XMLHttpRequest.jpg)

简称xhr，是浏览器提供的JavaScript对象，可以**请求服务器上的数据资源**

#### 使用xhr发起请求

**get请求**
* 创建xhr对象
* 调用`xhr.open()`方法创建请求
* 调用`xhr.send()`方法发起请求
* 监听xhr.onreadystatechange事件（可以拿到服务器响应的数据）

```html
<script>
	// 创建xhr对象
	var xhr = new XMLHttpRequest();
	// 调用open方法，指定请求方式和请求地址 ?后面拼接查询字符串（get请求的参数）
	xhr.open('GET', 'http://www.liulongbin.top:3006/api/getbooks?id=1');
	// 调用send方法，发送请求
	xhr.send();
	// 接收响应（监听onreadystatechange事件）
	xhr.onreadystatechange = function () {
		// 判断响应状态码  readyState(对象的请求状态) 4  status(服务器的响应状态) 200
		if (xhr.readyState === 4 && xhr.status === 200) {
			// 接收响应数据
			var res = xhr.responseText;
		}
	}
</script>
```

**`readyState`状态**

| 值   | 状态             | 说明                                            |
| :--- | :--------------- | :---------------------------------------------- |
| 0    | UNSET            | XMLHttpRequest对象已创建，但未调用`open()`方法  |
| 1    | OPENED           | 已调用`open()`方法                              |
| 2    | HEADERS_RECEIVED | `send()`方法已被调用，响应头也已被接收          |
| 3    | LOADING          | 数据接收中，此时response属性已加载 **部分数据** |
| 4    | DONE             | Ajax请求完成，数据传输已彻底的成功/失败         |

**查询字符串**	
* 在url末尾加上用于向服务器放松休息的字符串（data）
* 格式：`?参数1=值1&参数2=值2`如`?id=1&age=20`
	
**URL编码与解码**
* 在url地址中只允许出现英文相关字符，所以需要对其他字符进行转义，*用英文字符去表示非英文的字符*，浏览器会自动进行编码解码的过程
* `encodeURL()`编码函数
* `decodeURL()`解码函数

```js
// 编码     %E7%A7%A6%E5%A4%A9%E6%96%87
var qtw = encodeURI('秦天文');
console.log(qtw);
// 解码
console.log(decodeURI(qtw));
```

**post请求**
* 创建xhr对象
* 调用`xhr.open()`方法创建请求
* 设置 Content-Type 属性(请求头)
* 调用`xhr.send()`方法发起请求，同时指定发送的数据
* 监听xhr.onreadystatechange事件（可以拿到服务器响应的数据）

```js
$(function () {
	// 创建xhr对象
	var xhr = new XMLHttpRequest();
	// 设置请求方式和请求地址
	xhr.open('POST', 'http://www.liulongbin.top:3006/api/addbook');
	// 设置请求头
	xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
	// 发送请求
	xhr.send('bookname=快乐世界&author=Droliz&publisher=人民出版社');
	// 接收响应
	xhr.onreadystatechange = function () {
		if (xhr.readyState === 4 && xhr.status === 200) {
			console.log(xhr.responseText);
		}
	}
});
```

***数据交换格式***
* 服务器端与客户端之间进行数据传输与交换的格式，经常提及的有XML和JSON两种

* XML：E`X`tensible `M`arkup `L`anguage可扩展标记语言
	* XML与HTML没有任何关系，HTML是网页内容的载体，XML是数据的载体 
	* XML格式臃肿，和数据无关的代码过多，体积大，传输效率低在JS中解析比较麻烦
* JSON：`J`ava`S`cript `O`bject `N`otation，JavaScript对象表示法，它使用文本表示一个JS对象或数组的信息，JSON本质是字符串
	* JSON是一种轻量级的文本数据交换格式，但比XML更小、更快、更易解析

* JSON
	* JSON两种结构（对象，数组）
		* 对象结构：`{key: value, key: value}`key必须是**双引号**包裹的字符串，value可以是数字、字符串、布尔值、null、数组、对象（JSON中所有的字符串都用双引号包裹）
		* 数组结构：`["java", 1, true, null, {"name": "李四", "age": 18}, [1, 2, 3]]`取值可以是数字、字符串、布尔值、null、数组、对象

	* JSON的注意事项
		* 属性名、字符串必须要双引号包裹
		* JSON中不能写注释
		* JSON最外层必须是对象/数组格式
		* 不能使用undefined或函数作为JSON的值

	* JSON和JS对象的关系
		* JSON反序列化：将JSON字符串转换为JS对象，使用`JSON.parse()`方法
		* JSON序列化：将JS对象转换为JSON字符串，使用`JSON.stringify()`方法

>序列化：将数据对象转换为字符串的过程；反序列化：将字符串转换为数据对象的过程

```js
// 这是一个JS对象
var obj = {a: 'Hello', b: 'World'}
// 这是一个JSON字符串
var json = '{"a": "Hello", "b": "World"}'
// 将JSON字符串转换为JS对象
var obj1 = JSON.parse(json);
// 将JS对象转换为JSON字符串
var json1 = JSON.stringify(obj);
```

***XML新特性***
旧版的XML只支持文本数据的传输，无法用来读取和上传文件，而且传送和接收数据时没有进度信息，只能提示是否完成

**在新版中可以设置请求时限**，通过`timeout`属性设置，单位毫秒，当超过这个时限自动停止HTTP请求，通过`timeout`事件，设置回调函数

```js
// 设置超时时间
xhr.timeout = 3000;
xhr.ontimeout = function() { console.log("请求超时!); });
```

**可以使用FormData对象管理表单数据**

```js
// 模拟表单数据
// 创建 FormData 示例
var fd = new FormData();
// 调用 append 函数，向 fd 中追加数据
fd.append('uname', zs);
fd.append('age', 20);
// 使用 send 方法提交数据
xhr.send(fd);
```

```js
// 使用FromData对象获取表单值
var form = document.querySelector('form');
// 监听表单提交事件
form.addEventListener('submit', function(e){
	e.preventDefault();
	// 根据 form 表单创建 FormData 对象，会自动将表单中的数据填充到 FormData 对象中
	var formData = new FormData(form);
	var xhr = new XMLHttpRequest();
	xhr.open('POST', 'http://www.liulongbin.top:3006/api/formdata'); 
	xhr.send(formData);
	xhr.onreadystatechange = function(){
		if(xhr.readyState === 4 && xhr.status === 200){
				console.log(xhr.responseText);       
		}
	}
});
```

**可以上传文件**

```html
<body>
	<!-- 文件上传 -->
	<form>
		<input type="file" name="file" id="file">
		<input type="button" value="上传" id="upload">
		

	</form>

	<script>
		// 文件上传
		// 为按钮添加事件监听
		$('#upload').on('click', function() {
			// 获取用户选择的文件列表
			var files = document.querySelector('#file').files;
			// 判断是否有文件
			if(files.length <= 0) {
				alert('请选择文件');
				return;
			}
			// 创建表单对象
			var formData = new FormData();
			// 将文件加入表单
			formData.append('avatar', files[0]);
			// 创建ajax对象
			var xhr = new XMLHttpRequest();
			// 调用open方法
			xhr.open('POST', 'http://www.liulongbin.top:3006/api/upload/avatar');
			// 发送请求
			xhr.send(formData);
			// 监听ajax请求的状态
			xhr.onreadystatechange = function() {
				if(xhr.readyState == 4 && xhr.status == 200) {
					// 获取响应数据
					var res = xhr.responseText;
					var data = JSON.parse(res);

					if (data.status == 200) {
						alert('上传成功');
					} else {
						alert('上传失败');
						console.log(data.message);
					}
				}
			}
		})
	</script>
</body>
```

>用jQuery实现文件上传是，必须在Ajax的POST请求中添加两个属性，属性值必须为false，而且必须用`$.ajax()`请求
>`contentType: false, processData: false`分别代表：不修改Content-Type属性，使用FromData默认的值，不对FromData中数据进行url编码。而是原样发送到服务器

**可以获得数据传输的进度信息**

通过监听`xhr.upload.onprogress`事件，来获取文件的上传进度

```js
// 监听文件上传进度
xhr.upload.onprogress = function(e) {
	// 获取上传进度
	// e.loaded 已经上传的字节数
	// e.total 文件的总字节数
	var percent = Math.ceil((e.loaded / e.total) * 100) + '%';
	console.log(percent);
}
```

jQuery实现loading效果

```js
// jQuery 版本在1.8之后，只能附加到整个文档上，会监听文档内的所有Ajax请求

// 监听ajax请求开始
$(document).ajaxStart(function () {
	// 显示
	$('#loading').show();   
});
// 同样的，ajaxStop监听Ajax请求结束
$(document).ajaxStop(function () {
	$('#loading').hide();
})
```

### Axios

Axios是专注于**网络数据请求**的库，相比于原生的XMLHttpRequest对象，Axios简单易用，相比jQuery，axios更加轻量化，只专注于网络数据请求
        
**axios发起GET请求**

`axios.get('url', {params: {/* 参数 */} }).then(callback).catch(error);`

```js
// 使用axios发送GET请求
paramsObj = {name: 'zs', age: 20};
axios.get('http://liulongbin.top:3006/api/get', { params: paramsObj })
	.then(function(response){
		// 服务器返回的数据
		console.log(response.data);
	})
	.catch(function(error){
		// 请求失败处理
		console.log(error);
	});
```

**axios发起POST请求**

`axios.post('url', {/* 参数 */}).then(callback).catch(error);`

```js
// 使用axios发送POST请求
axios.post('http://www.liulongbin.top:3006/api/post', {
	location: '北京',
	address: '顺义'
}).then(function (response) {
	console.log(response.data);
}).catch(function (error) {
	console.log(error);
});
```

**直接使用axios发起请求**

```js
axios({
	method: "请求类型",
	url: "地址",
	data: {/* POST数据 */}
	params: {/* GET参数 */}
}).then(callback).catch(error);
```

---
### 跨域与JSONP

***同源策略与跨域***
* 同源：如果两个页面的**协议**、**域名**和**端口**都相同，则两个页面具有相同的源（端口未写默认为80）
* 同源策略：**浏览器**提供的一个**安全功能**
	* 限制了从同一个源加载的文档或脚本如何与来自另一个源的资源进行交互。这是一个用于隔离潜在恶意文件的重要机制
* 跨域：与同源相反
	* 出现跨域的根本原因：**浏览器的同源策略**不允许非同源的URL之间进行资源的交互
	* 浏览器对跨域请求的拦截

![浏览器对跨域请求的拦截](http://www.droliz.cn/markdown_img/浏览器对跨域请求的拦截.jpg)

>浏览器允许发起跨域请求，但是，跨域请求回来的数据，会被浏览器拦截，无法被网页获取

实现跨域请求：JSONP、CORS

#### JSONP
* Json with padding：是JSON的一种"使用模式"，**和ajax没有任何关系**
* 出现的早，兼容性好，是为了解决跨域问题而提出的**临时解决办法**
* **只支持GET请求**
* 原理：`<script>`标签不受浏览器同源策略的影响，可以通过scr属性，请求跨域的数据接口，并通过**函数调用的形式**（即将函数调用单独放过在一个js文件中，用查询字符串的形式在src后面指定callback的名字），接收跨域接口响应回来的数据

```html
<script>
function success(data) {
	console.log(data);
}
</script>

<script src="http://ajax.frontend.itheima.net:3006/api/jsonp?callback=success&name=ls&age=30"></script>
```

```js
// jQuery中的JSONP
$('.search-input').on('input', function(){
	var val = $(this).val();
	$.ajax({
		url: 'http://suggest.taobao.com/sug',
		data: {
			q: val
		},
		dataType: 'jsonp',
		success: function(res){
			console.log(res);
		}
	})
})
```

>默认情况下，使用jQuery发起JSONP请求，会自动携带一个 `callback=jQueryXXX`的参数`jQueryXXX`是书记生成的回调函数的名称

#### CORS

出现的比较晚，是W3C标准，属于Ajax请求跨域的**根本解决方案**，不兼容低版本浏览器

#### 防抖和节流

**防抖策略**

* 当事件被触发后，延迟 n 秒后再执行回调函数，如果再这 n 秒内事件又被触发，则重新计时（lol回城操作、搜索框连续输入）

```html
// 输入框的防抖效果
<script>
	// 防抖的timer
	var timer = null;
	// 定义防抖函数  keywords: 请求参数
	function debounce(keywords) {
		// 清除定时器
		clearTimeout(timer);
		// 定时器
		timer = setTimeout(function () {
			// 发送JSONP请求
			$.ajax({
				url: 'http://suggest',
				data: {
					wd: keywords
				},
				dataType: 'jsonp',
				success: function (data) {
					console.log(data);
				}
			});
		}, 500)
	}

	// 触发事件时调用防抖函数
	$('.search-input').on('keyup', function () {
		debounce(keywords);
	})
</script>
```

>每次搜索时，可以将结果放到一个对象中 `var = cacheObj = {}`，当再次请求时，可以先查看是否再缓存对象中存在(优先查看缓存)，如果有，那么直接从缓存对象中获取，避免多次重复的请求，提高效率

**节流策略**

* 减少一段时间内事件的触发频率，当鼠标连续不断的触发某事件，只在单位时间触发一次
* 节流阀：如同红绿灯，红灯表示被占用，绿灯表示可使用，具体见[JS](JS.md)    

区别

* 防抖：如果事件被频繁触发，防抖能保证**只有最后一次触发生效**，前面都被忽略
* 节流：如果事件被频繁触发，节流能够**减少事件触发的频率**，因此，节流是**有选择性的**执行一部分事件

## Git

### 起步

版本控制软件：用来记录文件变化，一边将来查阅特定版本修订情况的系统，因此有时也叫'版本控制系统'。操作方便、易于对比、易于回溯、不易丢失、协作方便

* 版本控制系统分类
	* 本地版本控制系统：**单机运行**，是维护文件版本的操作工具化
		* 使用软件的形式记录文件的不同版本，但**单机运行**，不支持多人协作开发、版本数据库如果发生故障，**所有历史版本都会丢失**
	* 集中化版本控制系统：联网运行，支持多人协作开发；**性能差，用户体验不好**
		* 基于服务器、客户端的运行模式，服务器保存文件的所有更新记录，客户端只保留最新的文件版本，可以多人协助，但**不支持离线**提交版本更新，中心服务器奔溃所有人无法正常工作，版本数据库故障后，所有历史更新记录会丢失
	* 分布式版本控制系统：联网运行，支持多人协作开发；性能优秀、用户体验好
		* 服务器保存文件的所有更新版本，客户端完整备份了服务器，支持多人协作，**客户端断网后，支持离线本地提交版本更新**，服务器损坏，可以用任意一个客户端的备份进行恢复

    * git
        * 开源的分布式版本控制系统
        * git特性
            * 直接记录快照，而非差异性比较

SVN的差异比较

![SVN的差异比较](http://www.droliz.cn/markdown_img/SVN差异比较.jpg)

传统的版本控制系统是**基于差异**的版本控制，他们存储的是**一组基本文件**和**每个文件随时间逐步积累的差异**（节省磁盘空间，但耗时、效率低）

Git的记录快照快照

![Git快照](http://www.droliz.cn/markdown_img/Git快照.jpg)

在原有文件版本的基础上重新生成一份文件，类似备份，如果文件没有修改，那么就不会重新存储（占用空间大，但版本切换快）

近乎所有的操作都是本地执行
断网依旧可以对项目进行版本管理，联网后本地记录就会同步到云端服务器

### git三种区域
使用Git管理项目，拥有三个区域，工作区、暂存区、Git仓库
* 工作区：处理工作的区域
* 暂存区：已完成的工作的临时存放区域，等待被提交
* Git仓库：最终存放区

### git三种状态
* 已修改（modified）：表示修改了文件，但还没将修改的结果放到暂存区
* 已暂存（staged）：表示对已修改文教的当前版本做了标记，使之包含在下次提交的列表中
* 已提交（committed）：表示文件已经安全的保存在本地的Git仓库中

* 基本的 Git 工作流程

![基本Git工作流程](http://www.droliz.cn/markdown_img/基本Git工作流程.jpg)
* 在工作区修改文件
* 将想要下次提交的更改进行暂存
* 提交更新，找到暂存区的文件，将快照永久性存储到Git仓库中

### git基础

[Git安装](http://git-scm.com/downloads)

Git全局配置文件在：C:\Users\lenovo 下  文件名为：gitcofig 可以查看曾经对git做过的全局配置

**终端命令***

查看全局变量文档
* 查看全部全局配置信息`git config --list --global`
* 查看指定的全局配置信息`git config user.name`

获取帮助文档
* 在浏览器（离线也可以）打开`git help <verb>`例如`git help config`
* 在终端中显示（简明的手册）`git <verb> -h`例如`git config -h`

获取Git仓库
* 将尚未进行版本控制的本地目录转换为 Git 仓库
	* 在项目根目录打开Git Bash
	* 执行`git init`命令将当前的目录转化为 Git 仓库
	
>会创建一个文件名为 .git 的隐藏目录，就是当前项目的 Git 仓库，里面包含了**初始的必要文件**，这些文件是 Git 仓库的**必要组成部分**
	
* 从其他服务器克隆一个已存在的 Git 仓库 
	* `git clone 远程仓库地址`

查看文件的状态            
* 工作区文件的四种状态

![工作区中文件的四种状态](http://www.droliz.cn/markdown_img/工作区中文件的4种状态.jpg)


查看文件所处状态：`git status`
* 以精简的方式显示：`git status --short`或`git status -s`

* ??（红色）：未跟踪
* A（绿色）：已跟踪，并处于暂存
* M（红色）：文件已修改（提交后）
* M（绿色）：已修改且已放入暂存区
* D（绿色）：表示这个文件在下次提交时（Git仓库中）删除

跟踪文件
	* `git add 文件`例如`git add index.html`(**还可以将已修改的文件放入暂存区**、**将有冲突的文件标记为已解决**)
	* `git add .`将新赠和修改过的文件都加入到暂存区，（所有的）
	
更新提交
* `git commit -m '描述'`例如`git commit -m '提交文件'`
* 跳过使用暂存区
	* Git 标准工作流程：工作区 -> 暂存区 -> Git仓库，但这样略显繁琐，此时可ui跳过暂存，工作区 -> Git仓库
	* 在更新提交后面添加 -a 选项，即可 `git commit -a -m "描述"`

>如果对已提交的文件进行更改，查询文件状态那么就会出现changes not staged for commit，说明文件的内容发生了变化，但还未放到暂存区 

撤销对文件的修改
* 将工作区中对应文件的修改，还原成 Git 仓库中保存的版本（**做的修改会丢失，且无法撤销**）
	* `git checkout -- 文件`例如`git checkout -- index.html`

将文件从暂存区移除
* `git reset HEAD 文件`例如`git reset HEAD index.html`改为.代表所有的

 移除文件
* 从Git仓库和工作区中同时移除
	* `git rm -f 文件`例如`git rm index.html`
* 只从Git仓库中移除文件
	* `git rm --cached 文件`例如`git rm --cached index.html`

忽略文件
* 创建一个文件名为 .gitignore 的配置文件，列出要忽略文件的匹配模式
* 格式规范
	* `#`开头的是注释
	* `/`结尾的是目录
	* `/`开头防止递归
	* `!`开头表示取反
	* 可以使用 glob 模式进行对文件和文件夹的匹配（glob 简化了的正则表达式）

* glob 模式
	* `*`匹配零个或多个
	* `[abc]`匹配任意一个列在方括号中的字符
	* `?`匹配任意一个字符
	* 方括号中使用短横线隔开，表示这两个字符范围内可以匹配
	* `**`表示匹配任意中间目录`a/**/z匹配a/z、a/b/z、a/b/d/z等`

查看提交历史
* `git log`按时间先后顺序列出，最近的在最上面
* `git log -2`展现最新提交的 2 条，数字可更改
* `git log -2 --pretty=oneline`在一行上显示最近提交的两条
* `git log -2 --pretty=format:"%h | %an | %ar | %s"`自定义输出格式
	* %h 提交的简写哈希值
	* %an 作者的名字
	* %ar 作者修订日期，按多久以前的方式显示
	* %s 提交说明

回退到历史版本
* `git reset --hard 历史的唯一标识`回退到指定的版本
* `git reflog --pretty=oneline`查看命令操作历史(在旧版本中向查看所有的操作，包括之前的新版本)

推送本地文件到平台
* `git remote add origin 请求方式HTTPS/SSH`将本地仓库内容和远程仓库内容连接，并将远程仓库命名为origin
* `git push -u origin master`将本地仓库内容推送到远程的origin仓库中（第二次及以后只用`git push`）

分支操作
* `git branch`查看当前仓库所有分支列表 前面有`*`代表当前分支
* `git branch 名称`在当前分支创建新分支，但还是处在当前分支，不会切换分支(新分支代码和当前主分支代码完全相同)
* `git checkout 分支名字`切换指定分支
* 分支的快速创建和切换
* `git checkout -b 分支名称`
* 合并分支
	* `git merge 分支名称`要先进入要合并的分支(如master)，再执行进行合并
* 删除分支
	* `git branch -d 分支名称`在非被删除分支上
* 将本地分支推送到远程仓库
	* `git push`如果是第一次推送需要加上`-u`参数表示关联`git push -u 远程仓库别名 本地分支名称:远程仓库分支名称`如果不写`:远程仓库分支名称`代表本地分支仓库名称和远程分支仓库名称相同（别名默认是 origin）
* 查看远程仓库所有分支信息
	* `git remote show 远程仓库名称`
* 跟踪分支
	* 从远程仓库，把对应的远程分支下载到本地仓库，保持本地分支和远程分支名字相同
	* `git checkout 远程分支名称`
	* `git checkout -b 本地分支名称 远程仓库名称/远程分支名称`如果需重命名
* 拉取分支
	* 从远程仓库，拉取当前分支最新的代码，保持当前分支的代码和远程分支代码的一致性
	* `git pull`
* 删除远程分支
	* `git push 远程仓库名称 --delete 远程分支名称`

### Github
* 开源
* 开放源代码，任何人都可以去查看、修改和使用开源代码

* 开源许可协议
* 为了**限制使用者的使用范围**和**保护作者的权利**，每个开源项目都应该遵守开源许可协议
* 常见5种开源协议
	* BSD(Berkeley Software Distribution)
	* Apache License 2.0
	* GPL(GNU General Public License)
		* 具有传染性的开源协议，不允许修改后和衍生的代码作为闭源的商业软件发布和销售（Linux）
	* LGPL(GNU Lesser General Public License)
	* MIT(Massachusetts Institute of Technology, MIT)
		* 目前限制在最少的协议，唯一条件在修改后的代码或者发行包中，必须包含原作者的许可信息（jQuery、Node.js）
		* 
* 开源项目托管平台
* GitHub
* Gitlab
* Gitee

* Github远程仓库
* 两种访问方式
	* HTTPS：**零配置**；但是每次访问仓库时，都需要输入账户和密码
	* SSH：需要进行额外配置；配置成功后每次访问仓库，不用输入账号密码
		* SSH key
			* 实现本地仓库和Github之间的免登录的加密数据传输
			* 组成
				* id_rsa(私钥文件，存放于客户端的电脑中即可)
				* id_rsa.pub(公钥文件，需要配置到Github中)

			* 生成SSH key
				* 在 Git Bash 中运行`ssh-keygen -t rsa -b 4096 -C"注册GitHub的邮箱"`

* ***git分支*** 
    * 类似平行宇宙，多个分支之间互不干扰，合并到一起时每个分支的功能都实现
    * 在多人协同时，防止互相干扰，提高效率
    ![分支](http://www.droliz.cn/markdown_img/分支.jpg)

    * master 主分支（main）
    * 在初始化本地 Git 仓库的适合，Git默认已经创建一个名字为 master 的主分支，用来**保存和记录**整个项目已完成的功能代码（不进行代码开发）

    * 功能分支
        * 专门用来开发新功能的分支，临时从master上分叉出来的，当新功能开发且测试完毕后，合并到master主分支上

    * 分支冲突
        * 如果在**不同的分支中对同一个文件进行了不同的修改**，Git就没法干净的合并他们，此时需要找到这些文件**手动解决冲突**

```bash
// 假设：将reg分支合并到master时，代码发生了冲突
git checkout master
git merge res

// 打开包含冲突的文件，手动解决冲突之后，再执行以下代码
git add .
git commit -m "解决分支合并冲突问题"
```