## BOM

### BOM基础

[window对象手册](https://www.runoob.com/jsref/obj-window.html)

BOM是 ***浏览器对象模型***，提供了独立于内容而与浏览器窗口进行交互的对象，其核心对象是window

BOM缺乏标准，JS语法的标准化组织是ECMA，DOM的标准化组织是W3C，BOM最初是Netscape浏览器标准的一部分

#### BOM构成
window对象是浏览器的顶级对象，它具有双重角色
* 他是JS访问浏览器窗口的一个接口
* 他是一个全局变量，定义在全局作用域中的变量、函数、都会变成window的对象方法

![BOM构成](http://www.droliz.cn/markdown_img/BOM构成.jpg)

>window下有一个特殊属性 window.name

### window对象的常见事件

窗口加载事件

```js
// 表示整个文件加载完才会触发（包括图像、脚本文件、CSS文件等）
window.onload = function() {代码块}（只能写一次）

window.addEventListener("load", function(){代码块})

document.addEventListener("DOMContentLoaded", function() {代码块})
```

>DOMContentLoaded是DOM加载完毕，不包括图片、CSS、flash等，就可以执行，比load快，load是将页面全部内容加载完毕才执行

调整窗口大小事件

```js
// 窗口大小发生变化时，就会调用
window.onresize = function() {}

window.addEventListener("resize", function() {代码块})
```

#### 定时器

* window中提供了两种定时器（方法）
* setTimeout(调用函数, [延迟毫秒数]) 
	* window可以省略
	* 当毫秒数结束时，调用函数（**只执行一次**）
	* 一般来说为了区分，都会给定时器起一个标识符（变量名）
	* ***回调函数***
		* 需要等待时间，时间到了才回去调用这个函数    
	* `clearTimeout(timeoutId)`可以终止定时器
* setInterval(函数, 时间间隔毫秒)
	* 每隔时间间隔调用一次（**重复调用**）
	* `clearInterval`可以清除定时器

```html
<!-- 验证码的间隔发送 -->
<script>
	var btn = document.querySelector('button');
	var input = document.querySelector('input');
	var placeholder = null;
	input.addEventListener('focus', function() {
		placeholder = this.placeholder;
		this.placeholder = "";
	})
	input.addEventListener('blur', function() {
		this.placeholder = placeholder;
	})

	btn.addEventListener('click', function() {
		this.disabled = true;
		var times = 60; 
		<!-- 标识符 -->
		var change_timer = setInterval(function() {
			if (times == 0) {
				btn.disabled = false;
				btn.innerHTML = '发送';
				clearInterval(change_timer);

			}else {
				btn.innerHTML = '还剩下' + times + 's';
				times--;
			}
		}, 1000);
	})
</script>    
```

#### this指向问题

在定义函数的时候this的指向是不确定的，只有函数执行的时候才能确定this的指向，一般情况下this的最终指向是调用它的对象

* 在**全局作用域**或**普通函数**中this指向全局对象window（在定时器指向window）
* 在方法调用中，this指向掉用者
* 构造函数中this指向构造函数的实例

#### JS执行队列

JS是单线程，同一时间只能做一件事.如：对于一个DOM元素进行添加和删除，只能先添加后删除，不能同时进行

![JS执行机制](http://www.droliz.cn/markdown_img/JS执行机制.jpg)


**同步**

* 第一个任务结束后，再执行后一个任务，程序的执行顺序与任务的排列顺序是一致的、同步的
* 同步任务：同步任务都在主线程上执行，形成一个 ***执行栈***

**异步**

* 在处理一个任务的时候，可以处理其他的任务
* 异步任务：JS的异步是通过 ***回调函数*** 实现的，放进任务队列中（通过异步进程处理判断是否放入队列，如点击事件在点击之后才会加入队列，定时器会在定时结束放入） 

>先执行执行栈中的同步任务，再将异步任务放入任务队列中，当执行栈中的所有同步任务执行完成，按次序读取任务队列中的异步任务，结束等待状态，进入任务栈中，开始执行

由于在主线程不断地重复获得任务、执行任务，这种机制称为**事件循环**（event loop）

#### location对象

用于获取或设置窗体的URL，并且可以用于解析URL

URL语法格式：`protocol://host[:port]/path/[?query]#fragment`
* protocol: 通信协议（http、ftp等）
* host: 主机（域名）
* port: 端口号
* path: 路径
* query: 参数，以键值对形式通过&分隔
* fragment: 片段，#后内容常见于链接锚点

示例

```html
<!-- 获取登录用户的参数 -->
// 登录界面
<form action="index.html">
	<input type="text" name="userId">
	<input type="submit" value="登录">
</form>

// 跳转界面
<script>
	// user = [userId, 输入的用户名] 
	var user = location.search.substr(1,).split('=');
	// 先获取键值对，再去掉开头的问号，然后以等号分隔字符串
</script>
```

| 方法               | 返回值                                                  |
| :----------------- | :------------------------------------------------------ |
| location.assign()  | 和href一样，也可以跳转，但记录历史，可以返回之前的页面  |
| location.replace() | 同assign，但不记录历史                                  |
| location.reload()  | 重新加载页面相当于F5，如果参数为true则强制刷新ctrl + F5 |

#### navigator对象

包含浏览器的相关信息，最常用的userAgent可以返回页面头部信息，可以根据头部信息，判断是跳转PC端页面还是PE端页面

#### history对象

主要是与浏览器历史记录进行交互，该对象包含用户再浏览器窗口中访问过的所有URL


### PC端页面特效

#### 元素偏移量

offset的相关属性可以**动态的**获取元素的位置、大小等

* 获得元素距离 ***带有定位*** 父元素的位置
* 获取元素自身的大小
* 所有获取的参数均不带单位

>style只能得到行内样式表中的样式值，offset可以得到任意样式值，获取元素的样式值用offset更合适，更改元素样式值用style更合适

应用：一般的想要得到鼠标再盒子中的位置，先得到鼠标在页面中的位置(e.pageX, e.pageY)再获取盒子再页面中的距离(box.offsetLeft, box.offsetTop)相减就可得到鼠标再盒子中的位置

应用分析: （拖拽盒子）    
* 先点击鼠标，再移动鼠标，然后放开鼠标，实现鼠标的移动.（先点击再移动，所以 mousemove 事件和 mouseup 事件放在 mousedown 事件中）
* 当鼠标落下时，获得鼠标再盒子内的坐标和鼠标再页面的坐标，相减就是盒子的坐标，赋值给盒子，当鼠标松开解除移动事件

应用分析：（图片局部放大）
* 局部放大图片先明确鼠标移动的范围，

#### 元素可视区
 
 client的相关属性可以获取元素可视区的相关信息，通过client的相关属性可以动态的获取元素的边框大小、元素大小等（相比offset，client宽度不包括边框的大小）

#### 立即执行函数

不需要调用，立即就能执行的函数，如下两种写法：

```js
(function() {}(
	// code
))

(function() {})(
	// code
)
```

主要用于创建一个独立的执行域，避免命名冲突的问题

#### 元素滚动

scroll相关属性可以动态获得该元素的大小、滚动距离等

***总结***

* offset系列经常用于获取元素的位置
* client系列经常用于获取元素的大小
* scroll系列经常用于获取滚动距离

    >当元素溢出时，可以通过overflow属性选择是否显示溢出内容

#### 动画

动画实现的原理
* 核心原理：通过定时器setInterval不断移动盒子的位置
* 1、获得盒子当前位置
* 2、让盒子当前位置上加上一个移动距离
* 3、利用定时器不断重复这个操作
* 4、加一个结束定时的条件
* 此元素需要添加定位，才能使用element.style.left

>对于一个封装好的动画函数，每次调用都会在内存中申请空间用于存储计时器，比较浪费资源，但可以将每个获取过来的对象添加一个属性，将定时器添加进去

```js
function(obj, target) {
	obj.name = setInterval(function() {
		// 定时器执行语句块
	}, 30);
}
```

**缓动效果原理**

缓动动画就是让元素运动速度有所变化，最常见的是让速度慢慢停下来。让盒子每次移动的距离慢慢变小

核心算法：（目标值 - 现在的位置） / 10 作为每次移动的距离步长(根据具体情况更改)

动画函数添加回调函数。回调函数原理：函数可以作为一个参数.将这个函数作为一个参数传到另一个函数里面当那个函数。执行完之后，再执行传进去的这个函数，这个过程叫做 ***回调***

==回调函数位置：定时器结束的位置==

```html
<div></div>
<button class="btn500px">500px</button>
<button class="btn800px">800px</button>
<script>
	function animate(obj, target, callback) {
		clearInterval(obj.timer);
		obj.timer = setInterval(function () {
			var step = Math.ceil((target - obj.offsetLeft) / 10)
			step = step > 0 ? Math.ceil(step) : Math.floor(step);

			if (obj.offsetLeft == target) {
				clearInterval(obj.timer);
				// 回调函数
				if (callback) {
					callback();
				}
			}
			obj.style.left = obj.offsetLeft + step + 'px';

		}, 15);
	}

	var div = document.querySelector("div");
	var btn_5 = document.querySelector(".btn500px");
	var btn_8 = document.querySelector(".btn800px");

	btn_5.addEventListener('click', function () {
		animate(div, 500, function () {代码块});
	});
	btn_8.addEventListener('click', function () {
		animate(div, 800, function () {代码块});
	});
</script>
```

>由于会经常使用动画，可以单独将动画封装到一个JS文件中，需要使用的时候调用即可

#### 节流阀
当上一个函数执行完毕时再去执行下一个函数，避免事件连续触发太快

核心实现思路：利用回调函数，添加一个变量来控制，锁住函数以及解锁函数

再动画中设置变量 flag = true，执行时变为false 此时禁止下次动画，回调函数执行时将flag = true

---

### 本地存储

**简介**

* 将数据存储再用户的浏览器中
* 设置、读取方便，甚至页面刷新也不会丢失数据

#### localStorage

localStorage 存放数据，即便浏览器关闭，数据也不会消失

```js
// 保存一个数据
localStorage.setItem(key, value)

// 获取一个数据
localStorage.getItem(key)

// 删除
localStorage.removeItem(key)

// 清空
localStorage.clear()
```

值得注意的是在保存时，会自动调用 `toString()` 方法，所以如果保存的是对下个类型的数据，可以使用 `JSON.stringify(value)` 解决，读取数据是JSON字符串需要 `JSON.parse()` 方法解析

#### sessionStorage

`localStorage` 中的方法在 session 中也有，但相较于上者，这个关闭浏览器就会消失
