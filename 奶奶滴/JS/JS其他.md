## 其他

### 原型、原型链


![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220903173626.png)

### sql注入

#### 防止输入框sql注入

```js
// 防止输入框 sql 注入
preSql = function (obj){
	var dom = $(obj);
	var re = /select|update|delete|exec|count|'|"|=|;|>|<|%/i;
	if (re.test(dom.val().toLowerCase())){
		alert("请勿输入非法字符！");
		dom.val("");
	}
}

inputs = $('input');
// console.log(inputs);

inputs.each(function(index, item){
	$(item).on('input', function(){
		preSql(this);
	});
});
```


### 防抖和节流

防抖：在事件被触发 n 秒后再执行回调，如果在这 n 秒内又被触发，则重新计时
-   给按钮加函数防抖防止表单多次提交。
-   对于输入框连续输入进行AJAX验证时，用函数防抖能有效减少请求次数。
-   判断scroll是否滑到底部，滚动事件+函数防抖

节流：指定时间间隔 n 内只会执行一次任务。
-   游戏中的刷新率
-   DOM元素拖拽
-   Canvas画笔功能

**防抖**
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>防抖</title>
</head>
<body>
  <button id="debounce1">点我防抖！</button>

  <script>
    window.onload = function() {
      // 1、获取这个按钮，并绑定事件
      var myDebounce = document.getElementById("debounce1");
      myDebounce.addEventListener("click", debounce(handle);
    }

    // 2、防抖功能函数，接受传参
    function debounce(fn) {
      // 4、创建一个标记用来存放定时器的返回值
      let timeout = null;
      return function() {
        // 5、每次当用户点击/输入的时候，把前一个定时器清除
        clearTimeout(timeout);
        // 6、然后创建一个新的 setTimeout，
        // 这样就能保证点击按钮后的 interval 间隔内
        // 如果用户还点击了的话，就不会执行 fn 函数
        timeout = setTimeout(() => {
          fn.call(this, arguments);
        }, 1000);
      };
    }

    // 3、需要进行防抖的事件处理
    function handle() {
      // 有些需要防抖的工作，在这里执行
      console.log("防抖成功！");
    }

  </script>
</body>
</html>
```

**节流**
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>节流</title>
</head>
<body>

  <button id="throttle">点我节流！</button>

  <script>
    window.onload = function() {
      // 1、获取按钮，绑定点击事件
      var myThrottle = document.getElementById("throttle");
      myThrottle.addEventListener("click", throttle(sayThrottle));
    }

    // 2、节流函数体
    function throttle(fn) {
      // 4、通过闭包保存一个标记
      let canRun = true;
      return function() {
        // 5、在函数开头判断标志是否为 true，不为 true 则中断函数
        if(!canRun) {
          return;
        }
        // 6、将 canRun 设置为 false，防止执行之前再被执行
        canRun = false;
        // 7、定时器
        setTimeout( () => {
          fn.call(this, arguments);
          // 8、执行完事件（比如调用完接口）之后，重新将这个标志设置为 true
          canRun = true;
        }, 1000);
      };
    }

    // 3、需要节流的事件
    function sayThrottle() {
      console.log("节流成功！");
    }

  </script>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>节流</title>
</head>
<body>

  <button id="throttle">点我节流！</button>

  <script>
    window.onload = function() {
      // 1、获取按钮，绑定点击事件
      var myThrottle = document.getElementById("throttle");
      myThrottle.addEventListener("click", throttle(sayThrottle));
    }

    // 2、节流函数体
    function throttle(fn) {
      // 4、通过闭包保存一个标记
      let canRun = true;
      return function() {
        // 5、在函数开头判断标志是否为 true，不为 true 则中断函数
        if(!canRun) {
          return;
        }
        // 6、将 canRun 设置为 false，防止执行之前再被执行
        canRun = false;
        // 7、定时器
        setTimeout( () => {
          fn.call(this, arguments);
          // 8、执行完事件（比如调用完接口）之后，重新将这个标志设置为 true
          canRun = true;
        }, 1000);
      };
    }

    // 3、需要节流的事件
    function sayThrottle() {
      console.log("节流成功！");
    }

  </script>
</body>
</html>
```


### Object 的方法

#### defineProperty

接收三个参数
* 属性所在对象
* 属性名字
* 描述符对象
	* configurable：表示能否通过delete删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性，默认值为false
	* enumerable：表示能否通过for in循环访问属性，默认值为false
	* writable：表示能否修改属性的值。默认值为false
	* value：包含这个属性的数据值。默认值为undefined
	* setter与getter

### 数组的方法

#### reduce

reduce() 方法接收一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终计算为一个值

**注意:** reduce() 对于空数组是不会执行回调函数的

例如：计算数组中偶数个数

```js
let arr = [1, 2, 3, 4, 5, 6]
  
// pre为上一个回调函数的返回值，current为每个元素
const n = arr.reduce((pre, current) => {
    console.log(pre, current);
    return pre + (current % 2 ? 1 : 0)
}, 0)   // pre 初始值
  
console.log(n);
```