# ES6模块化与异步编程高级用法

## ES6模块化

### 简介
在ES6之前已经有社区提出AMD、CMD、CommonJS等模块化，但都不是浏览器与服务器通用的模块化标准

ES6为官方提出的统一的模块化规范

ES6 模块化规范中定义：
* 每个JS文件都是一个独立的模块
* 使用 `import` 关键字导入其他模块
* 使用 `export` 关键字向外共享模块

### ndoe.js使用ES6

需要node版本高于14.15.1，在package.json根节点中添加`"type": "module"`，type默认值是CommonJS

![[Pasted image 20220329173529.png]]

### 默认导出导入

`export default 默认导出成员`

```js
let n1 = 10
let n2 = 20

function test() {
 console.log(n1 + n2)
}
  
export default {

 n1, test

}
```

`import 别名 import 模块标识符`

```js
import test from './test.js'

console.log(test)		// 打印一个包含导出成员的对象

console.log(test.n1)		// 10
console.log(test.n2)		// undefined
```

>一个模块只能默认导出一次（只能有一个export default）

### 按需导入导出

`export 成员`，按需导出可以有多个

`import { 成员1 as 别名, 成员2, 成员3 …… } from 模块`这里的成员名称必须和导出的名称一致，可以使用as重命名

### 直接导入并执行模块代码

`import 模块标识符`会直接执行模块的代码


## Promise

### 回调地狱

多层回调函数相互嵌套，就形成了回调地狱
* 代码的耦合性太强，难以维护
* 大量冗余的代码相互嵌套，导致代码可读写差

ES6中通过Promise解决回调地狱的问题

### 基本概念

Promise是一个构造函数
* 创建promise实例 `const p = new Promise()`
* 每一个new出来的promise实例对象，代表一个异步操作

`Promise.prototype`上包含`then()`方法
* 可以通过原型链的方式访问到`then()`方法
* `then()`方法用来预先指定成功（result => {}）和失败（error => {}）的回调函数
	* 成功的回调函数必选，失败的回调函数可选

```js
const p = new Promise();

p.then(result => {
	console.log("成功");
}, error => {
	console.log("失败");
});
```

#### 实例

异步读取文件（官方fs不支持promise，需要安装then-fs包）`npm install then-fs`

```js
import thenFs from "then-fs";
// 无法保证文件读取的顺序
thenFs.readFile("111.txt", 'utf8').then( r1 => { console.log(r1); } );
thenFs.readFile("222.txt", 'utf8').then( r2 => { console.log(r2); } );
thenFs.readFile("333.txt", 'utf8').then( r3 => { console.log(r3); } );
```

确保有顺序的读取（链式操作）

`Promise.prototype.catch`方法可以捕获和处理异步错误

```js
import thenFs from "then-fs";

thenFs
.readFile("./test_txt/111.txt", 'utf8')
.cacth(err => {		// 捕获前面的异步错误，防止出错误导致后面的then方法不能运行
 console.log(err.message);
});
.then((r1) => {
 console.log(r1);
 return thenFs.readFile("./test_txt/222.txt", 'utf8');
})
.then((r2) => {
 console.log(r2); 
 return thenFs.readFile("./test_txt/333.txt", 'utf8');
})
.then((r3) => {
 console.log(r3);
});
```


### Promise方法

#### Promise.all()
Promise.all()会发起并行的异步操作，等所有的操作结束才会继续执行后面的then()

等待机制
```js
import thenFs from "then-fs";

const promiseArr = [
 thenFs.readFile("./test_txt/111.txt", 'utf8'),
 thenFs.readFile("./test_txt/222.txt", 'utf8'),
 thenFs.readFile("./test_txt/333.txt", 'utf8'),
]

// 读取的顺序与数组顺序一致
Promise.all(promiseArr)
 .then(data => {
 console.log(data);
 });

// 输出
// 111
// 222
// 333
```

#### Promise.race()
与all()类似，但是race()只要有任何一个异步操作完成，都会立即执行下一个then()

赛跑机制
```js
import thenFs from "then-fs";

const promiseArr = [
 thenFs.readFile("./test_txt/111.txt", 'utf8'),
 thenFs.readFile("./test_txt/222.txt", 'utf8'),
 thenFs.readFile("./test_txt/333.txt", 'utf8'),
]

Promise.race(promiseArr)
 .then(data => {
 console.log(data);
 });

// 只会获得最快的那个
```

## async/await

asyanc 和 await 在es8中引入用于简化 Promise 异步操作，在此之前，只能通过`链式.then()的方式`处理 Promise 异步操作

### 基本使用

用await修饰后返回值不再是Promise实例，而是文件的内容，而**使用await的方法必须用async修饰**

```js
import thenFs from "then-fs";

async function getAllFile() {

    const files1 = await thenFs.readFile("./test_txt/111.txt", "utf8");
    console.log(files1);
    // output   111

    const files2 = await thenFs.readFile("./test_txt/222.txt", "utf8");
    console.log(files2);
    // output   222
  
    const files3 = await thenFs.readFile("./test_txt/333.txt", "utf8");
    console.log(files3);
    // output   333
}

getAllFile();
```

 注意：使用async修饰的方法中第一个await之前的代码会同步执行，之后的代码会异步执行

 示例：

~~~js
import thenFs from "then-fs";

console.log("A");
async function getAllFile() {
    console.log("B");  // 同步执行
    // 异步执行
    const files1 = await thenFs.readFile("./test_txt/111.txt", "utf8");
    const files2 = await thenFs.readFile("./test_txt/222.txt", "utf8");
    const files3 = await thenFs.readFile("./test_txt/333.txt", "utf8");
    console.log(files1, files2, files3);
    console.log("D");
}

getAllFile();
// 同步执行
console.log("C");

// output
// A
// B
// C
// 111 222 333
// D
// 主线程在调用函数时，发现后面的都是异步，所以先加入到任务队列然后退出后续异步的执行，执行主线程的同步操作，最后当所有的同步操作完成，按照任务队列执行异步操作
~~~


## EventLoop

### 同步任务与异步任务

* js是单线程的，为了防止一个程序时间过长，导致出现程序假死的情况，js将任务分为两个
	* 同步任务（synchronous）
		* 非耗时任务，在主线程上排队执行的任务
		* 只有前一个同步任务完成才能进行下一个
	* 异步任务（asynchronous）
		* 耗时任务，由js委托给宿主环境（浏览器、nodejs等）进行的任务
		* 当异步任务执行完，会通知js主线程执行异步任务的回调函数

### 执行过程

![](../markdown_img/Pasted%20image%2020220613234824.png)

* 同步任务由主线程按照顺序一次执行
* 异步任务委托给宿主环境执行，然后将对应的**回调函数加入到任务队列**中
* 当执行栈清空，然后按顺序将执行任务队列中的回调函数加入到栈
* 按照栈的顺序依次执行

### 宏任务与微任务

![](../markdown_img/Pasted%20image%2020220614000102.png)

* 在异步任务中划分两类
	* 宏任务（macrotask）
		* 异步ajax请求
		* 定时器
		* 文件操作
		* 其他
	* 微任务（microtask）
		* promise.then、.catch、.finally
		* process.nextTick
		* 其他

#### 宏任务与微任务执行

![](../markdown_img/Pasted%20image%2020220614001610.png)

交替执行：每一个宏任务执行完，都会检查是否由微任务，如果有，先执行所有微任务，然后继续下一个宏任务

示例：

~~~js
// 宏任务
setTimeout(() => {
    console.log("1");  

});

new Promise((resolve) => {
	// 同步任务
    console.log("2");
    resolve();
}).then(() => {
	// 微任务
    console.log("3");
});

// 同步任务
console.log("4");

// output
// 2
// 4
// 3
// 1
~~~

### 综合示例

~~~js
console.log("1");

setTimeout(() => {
    console.log("2");
    new Promise((resolve, reject) => {
        console.log("3");
        resolve();
    }).then(() => {
        console.log("4");
    })
})
  
new Promise((resolve, reject) => {
    console.log("5");
    resolve();
}).then(() => {
    console.log("6");
})
 
setTimeout(() => {
    console.log("7");
    new Promise((resolve, reject) => {
        console.log("8");
        resolve();
    }).then(() => {
        console.log("9");
    });
});
  

// output
// 1
// 5
// 6
// 2
// 3
// 4
// 7
// 8
// 9
~~~

## API接口案例

### 需求

基于 Mysql 数据库 + Express 对外提供用户列表的 API 接口

技术：
* express 与 mysql2
* ES6 模块化
* Promise
* async/await

### 项目结构
~~~
│  app.js  // 
│  package-lock.json
│  package.json
│
├─.idea
│
├─node_modules
│
├─bin  // 程序入口
│      www.js  // 
│
└─src
    ├─conf   // 相关配置文件
    │      db.js  // 数据库配置
    │
    ├─controller   // 实际业务
    │      user.js   // user业务逻辑
    │
    ├─db   // 数据库操作
    │      mysql.js  // 数据库执行
    │
    ├─model  // 数据返回格式
    │      resModel.js  // 数据模型
    │
    └─router  // 路由
            user.js  // user路由
~~~

### 业务模块

### 路由模块


