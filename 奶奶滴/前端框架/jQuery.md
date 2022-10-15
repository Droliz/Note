# jQuery

[jQuery API文档](https://jquery.cuishifeng.cn/index.html)

## JavaScript库
* 是一个封装好的特定的集合（方法和函数）
* jQuery库是为了快速方便的操作DOM 
* 常见的JS库
	* jQuery
	* Prototype
	* YUI
	* Dojo
	* Ext JS
	* 移动端的zepto

## jQuery的基本使用

### jQuery的入口函数

```js
方法一：
$(function () {
	// 页面DOM加载完成的入口
})
方法二：
$(document).ready(function () {
	// 页面DOM加载完成的入口
})
```

### jQuery顶级对象 $ 

$ 是 jQuery的别称，同时也是 jQuery的顶级对象

使用原生JS方法获取来的对象就是DOM对象`var div = document.querySelector('div');`

使用jQuery方法获取来的对象就是jQuery对象`var div = $('div');`

>jQuery对象的本质就是利用$对DOM对象进行包装后产生的新对象

在jQuery中有很多方法并没有包装，但是可以将DOM对象中的方法转为jQuery对象的方法

将DOM转换为jQuery`$(DOM对象)`直接获取DOM对象

将jQuery转换为DOM`$(DOM对象)[index]`或`$(DOM).get(index)`

### jQuery常用API


原生JS获取元素的方式比较繁琐，英尺jQuery对获取元素统一标准`$("选择器")`选择器同CSS中的选择器

遍历DOM元素的过程叫做**隐式迭代**

```js
// 排他
$(this).css('color', 'pink')
$(this).sibling().css('color', '')

// 简写
$(this).css('color', 'pink').sibling().css('color', '')
```

#### 样式操作

**设置样式**

`$("选择器").css("属性名", "值")`（必须加引号，必须逗号隔开，如果是数字不用写单位引号），如果有多个样式要设置，用键值对的形式填写每一组样式

`$('div').css('backgroundColor', 'pink')`给所有的div添加css方法（**隐式迭代**）

* 注意：`$("div").css("color")`返回的是属性值，只有填写了属性值才是修改属性值

>符合属性的属性名按照驼峰命名法

* **设置类样式方法**、
	* 1、添加类`$('元素').addClass('类名')`
	* 2、删除类`$('元素').removeClass('类名')`
	* 3、切换类`$('元素').toggleClass('类名')`如果没有就添加，如果没有就删除

* **类操作与原生JS的className的区别**
	* JS原生的className会覆盖原先的类名
	* 类操作只是追加/去掉一个类，不影响之前的类

* 效果
	* jQuery封装了很多动画效果
		* 显示隐藏                
		* 滑动
		* 淡入淡出
		* 自定义

>具体查阅[jQuery API文档](https://jquery.cuishifeng.cn/index.html)

* 事件切换：`hover(函数1, 函数2)`鼠标经过执行函数1，移开执行函数2(如果只写一个，经过移开都触发)
* 为了解决短时间触发多次动画而带来的排队，可以使用`stop()`方法停止上一次动画，来保证只有一个动画，stop方法必须写在动画前面

* 属性操作
* `prop('属性'[, '更改属性'])`（获取/更改元素**固有**属性）
* `attr('属性'[, '更改属性'])`（获取/更改元素**自定义**属性）

* 数据缓存`data('属性'[, '更改属性'])`方法在指定元素上存取数据(在缓存中)，不会更改DOM元素结构，页面刷新就会被移除

>用data()获取h5的数据,属性名不用加上data-,而且获取的返回值是数字型

* 文本内容值
* **获取/设置元素内容相当于原生中的innerHTML**           
	* `html('更改内容')`如果不填写代表获取内容
* **获取/设置元素文本内容相当于原生中的innerText**
	* `text('更改内容')`   
* **获取/更改表单的值相当于原生中的value**       
	* `val('更改内容')`

* 元素操作
* **遍历元素**
>针对同一元素的不同操作，可以用each()方法遍历

```js
// 遍历指定元素
$('元素').each(function (index, domEle) {
	// domEle要在jq中使用必须转换
	$(domEle).css('color', 'blue');
})
index:索引号
domEle:遍历内容（是DOM的元素对象）

// 遍历任意元素
$.each(object, function (index, ele) {
	代码块
})
object可以是元素等任意对象皆可遍历
```

* **创建、添加、删除元素**
	* 创建`var div = $('<div?>div元素</div>')`
	* 添加
		* 内部添加`$('父元素').append('添加元素')`
			* append末尾添加
			* prepend开头添加
		* 外部添加`$('兄弟元素').after('添加元素')`
			* after末尾添加
			* before开头添加
	* 删除`$('元素').remove()`
		* remove删除自己
		* empty删除子元素
		* html('')直接修改内容
		
* 尺寸、位置操作
* **尺寸方法**
	* `$('元素').width('更改值')`
		* width/height：获取/设置width/height的大小
		* innerWidth/innerHeight：获取/设置width/height + padding的大小
		* outerWidth/outerHeight：获取/设置width/height + padding + border的大小(值为true时还加上margin默认为false)

* **位置方法**
	* `$('元素').offset()`
		* `offset().left/top`分别是获取左边和顶部对于**文档**的偏移
		* `offset({top: 值, left: 值})`设置值，不带单位

	* `$('元素').position()`
		* 获取元素对于**带有定位的父级**的偏移（只能获取）

	* scrollTop()/scrollLeft()设置/获取元素被卷去的头部和左侧

* ***jQuery事件***

    * **事件注册**
        * `$('元素').事件(function () {})`给所有指定的元素添加指定的事件(不加on比如click)
    * **绑定事件**
        * `on()`方法可以给指定元素绑定一个或多个事件
        * `$('元素').on({事件: 处理函数, 事件: 处理函数})` 
        * `on()`方法可以实现事件的委派
            * `$('父元素')on('事件', '子元素', function () {})`此时注册在子元素上的事件，父元素也有 
        * `on()`方法可以动态的绑定事件（比如后来创建的元素）
    
模拟微博发布

```html
<script>
	// 入口函数
	$(() => {
		// 1、点击发布按钮，动态创建一个li，放入文本框的内容和删除按钮，并且清空文本框，添加到ul中
		// 必须用on()创建
		$(".btn").on('click', () => {
			// 创建li
			var li = $("<li></li>");
			li.html($(".text").val() + '<button class="del">删除</button>'); 
			// 添加到ul中
			$("ul").prepend(li);
			// 清空文本框
			$(".text").val("");
		});

		// 2、点击删除按钮，删除当前li
		$("ul").on('click', 'button', () => {
			$(this).parent().remove();
		});

	});
</script>

<body>
	<div class="box" id="weibo">
		<span>微博发布</span>
		<textarea name="" id="" cols="30" rows="10" class="text"></textarea>
		<button class="btn">
			发布
		</button>
		<ul>  
		</ul>
	</div>
</body>
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>发表评论</title>

	<script src="http://www.droliz.cn/JavaScript/lib/jQuery.js"></script>
	<script src="http://www.droliz.cn/JavaScript/lib/bootstrap-3.4.1-dist/bootstrap.js"></script>
	<link rel="stylesheet" href="http://www.droliz.cn/css/lib/bootstrap-3.4.1-dist/bootstrap.css">

	<script>
		// 入口函数
		$(function () {
			// 给发表按钮添加事件监听
			$("button").on('click', function () {

				//封装一个函数，用来将数据发表到评论区
				var f = function () {
					// 获取输入框的内容
					var content = $("#content").val().trim();
					var name = $("#name").val().trim();
					// 获取当前的时间
					var time = new Date();
					var year = time.getFullYear();
					var month = time.getMonth() + 1;
					var day = time.getDate();
					var hour = time.getHours();
					var minute = time.getMinutes();

					// 判断内容是否为空
					if (!content || !name) {
						alert("请输入内容");
					}

					// 将内容放到评论区的ul列表中
					// 格式：<li class='list-group-item'><span>内容</span><span class="badge">时间</span><span class="badge">用户名</span></li>
					var str = "<li class='list-group-item'><span>" + content + "</span><span class='badge' style='background-color: #F0AD4E;'>" + year + "-" + month + "-" + day + " " + hour + ":" + minute + "</span><span class='badge' style='background-color: #5BC0DE;'>" + name + "</span></li>";

					// 将内容放到评论区的ul列表中的最上面
					$("#comment-ul").prepend(str);

					// 清空输入框
					$("#content").val("");
					$("#name").val("");
				}
				// 调用函数
				f();

			});
		});
	</script>


</head>

<body style='padding: 15px;'>
	<!-- 发表评论面板 -->
	<div class="panel panel-default">
		<div class="panel-heading">
			<h3 class="panel-title">发表评论</h3>
		</div>
		<div class="panel-body">
			<!-- 发表评论 -->
			<!-- 发表人 -->
			<div class="form-group">
				<label for="name">发表人</label>
				<input type="text" class="form-control" id="name" placeholder="请输入发表人">
			</div>
			<!-- 发表内容 -->
			<div class="form-group">
				<label for="content">发表内容</label>
				<textarea class="form-control" id="content" rows="3"></textarea>
			</div>
			<!-- 发表按钮 -->
			<button type="submit" class="btn btn-default">发表</button>

		</div>
	</div>

	<!-- 评论列表 -->
	<div class="panel-body">
		<ul class="list-group" id="comment-ul">
			<!-- 动态获取添加数据 -->

		</ul>
	</div>



</body>

</html>
```
    
* **事件解除**
	* `$('元素').off('事件')`默认解除所有事件
	* `$('父元素').off('事件', '元素')`解除在元素的事件委托

>用`one('事件', function () {})`方法绑定事件可以实现只触发一次，不用解绑

* **自动触发事件**
	* `$('元素').事件()`
	* `$('元素').trigger('事件')`
	* `$('元素').triggerHandler('事件')`不会触发元素的默认行为(光标闪烁等)

* **事件对象**
	* `event.preventDefault()`可以阻止默认行为
	* `event.stopPropagation()`可以阻止冒泡

* ***jQuery对象的拷贝方法***
    * `$.extend([deep], target, object1, [objectN])`
        * deep：如果设为true为深拷贝，默认为false浅拷贝(浅拷贝是拷贝的地址,深拷贝会拷贝所有，新开一个独立的对象)
        * target：要拷贝的目标对象
        * objectN：待拷贝的第N个对象

```js
var obj = {
	name: 'andy',
	id: 1;
	sex: '男'
	}

$.extend(true, targetObj, obj)如果targetObj有数据，会覆盖
```

* ***jQuery多库共存***
    * 让jQuery和其他的js库不存在冲突，可以同时存在
    * 1、将jQuery中的$换为jQuery
    * 2、`$.noConflict()`让jQuery释放对$的控制权，让用户控制

* ***jQuery插件***
    * [jQuery插件库](http://www.jq22.com/)
    * [jQuery之家](http://www.htmleaf.com/)
