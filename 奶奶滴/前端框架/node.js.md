# node.js

### 认识node.js和内置模块

* ***认识node.js***
    * node.js 是一个基于 Chrome V8 引擎的 JavaScript **运行环境**
    
    * 浏览器是 JS 的**前端运行环境**
    * node.js 是 JS 的**后端运行环境**
    * node.js无法调用 DOM 和 BOM 等浏览器内置 API
    [Node.js官网](https://nodejs.org/zh-cn/)

    * node.js作为 JS 的运行环境，也仅仅提供了基础功能，但很多强大的工具和框架层出不穷
        * 基于[Express 框架](https://www.expressjs.com.cn/)，可以快速构建 Web 应用
        * 基于[Electron 框架](https://www.electronjs.org/)，可以构建跨平台的桌面应用
        * 基于[restify 框架](http://www.restify.com/)，可以快速构建 API 接口项目

* ***Node.js 学习路径***
    * JS基础语法 + Node.js 内置 API模块 + 第三方 API 模块

* ***fs文件系统模块***
    * `const fs = require('fs')`导入 fs 模块
    
    * 读取文件
        * `fs.readFile(path[, options], callback)`(路径, 编码格式, 回调)
        * `function(err, dataStr){}`中如果读取失败err=null，如果读取成功err的值为错误对象，dataStr=undefined，可以通过err值判断文件是否读取成功

    * 写入内容
        * `fs.writeFile(file, data[, options], callback)`(如果没有此文件，会创建文件并写入)

* ***path路劲模块***
    * `const fs = require('path')`

    * 路径拼接
        * `path.join([http://www.droliz.cn.paths])`将多个路径片段拼接为完整的路径字符串
        * `file_path = path.join(__dirname, './Ajax.md')`

>`__dirname`表示当前文件所处的目录

* 获取路径中的文件名
	* `path.basename(file_path[, ext])`获取路径中的文件名（ext：扩展名）
	* `path.basename(path.join(__dirname, './Ajax.md'), '.md')`如果写了后缀，返回值就不会带后缀

* 获取路径文件扩展名
	* `path.extname(file_path)`获取路径中文件的扩展名
	* `path.extname(path.join(__dirname, './Ajax.md'))`

* ***http模块***
    * 由Node.js官方提供，用来**创建 web 服务器**的模块，通过`http.createSServer()`方法可以很方便地将一台普通的电脑，变成3一台Web服务器，从而对外提供Web资源服务

    * 服务器上安装了 **web 服务软件**，例如 IIS、Apache 等，通过安装这些服务器技术就能把一台普通电脑变成一台 web 服务器，但在Node.js中可以简单的对外提供web服务

    * IP地址
        * 互联网上每台计算机的**唯一**地址，只有找到对方的IP地址的前提下，才能进行数据通信
        * 以'点分十进制'表示成（a.b.c.d）的形式，都在0-255之间

    * 域名、域名服务器
        * 一个IP会有对应的域名，相比通过IP访问，通过域名地址更为直观，方便记忆
        * IP和域名的对应关系存放在 **域名服务器**（DNS，Domain name server）上，对应的转换工作在域名服务器上实现，**域名服务器就是提供 IP 地址和域名之间的转换的服务器**

    * 端口号
        * 服务器上有着成百上千的 web 服务，每个 web 服务对应**唯一的**端口号，通过端口号，可以可以被准确的交给对应的 web 服务进行处理 (80端口可以被省略)

    * 创建最基本的web服务器
        * 导入模块、创建web服务器实例、为服务器绑定 request 事件，监听客户端的请求、启动服务器

```js
// 导入 http 模块
const http = require('http');
// 创建服务器实例对象
let server = http.createServer();
// 监听客户端请求       req:请求对象  res:响应对象
server.on('request', function (req, res) {
	// 设置响应头
	res.writeHead(200, {
		'Content-Type': 'text/html;charset=utf-8'
	});
	// 设置响应内容
	var str = `你的url是: ${req.url}`
	// 添加响应内容
	res.write(str);
	// 发送响应内容
	res.end();
});
// 启动服务器
server.listen(9909, () => {
	console.log('服务器启动成功，端口号：9909');
});   

// 关闭服务器 (在终端中按 Ctrl + c)
// server.close();
```

* 根据不同的URL响应不同的内容
	
	* 获取请求的url、设置默认的响应内容为404 Nor Found、根据用户请求的url响应不同的内容

```js
// 导入 http 模块
const http = require('http');
// 创建服务器实例对象
const server = http.createServer();
// 监听客户端请求       req:请求对象  res:响应对象
server.on('request', function (req, res) {
	// 设置响应头
	res.writeHead(200, {
		'Content-Type': 'text/html;charset=utf-8'
	});
	// 设置默认响应内容为 404
	let result = '404';
	// 判断请求路径
	switch (req.url) {
		case '/':
		case '/index.html':
			result = '首页';
			break;
		case '/about':
			result = '关于我们';
			break;
		case '/news':
			result = '新闻';
			break;
		case '/contact':
			result = '联系我们';
			break;
		default:
			result = '404';
			break;
	}
	// 响应内容
	res.end(result);
});
// 启动服务器       es6语法
server.listen(9909, () => {
	console.log('服务器启动成功，端口号：9909');
});   

// // 关闭服务器 (终端 Ctrl + C )
// server.close();
```

### 模块化

* ***模块化基本概念***
    * **模块化**是指解决一个复杂问题时，自顶向下逐层把系统划分成若干个模块的过程，对于整个系统而言，模块是可组合、分解和更换的单元
        * 提高代码的复用性、可维护性
        * 可以按需加载

    * **模块化规范**
        * 对代码进行模块化的拆分与组合时，需要遵守的规则（可以降低沟通成本，加大模块之间的互相调用）

    * **Node.js中的模块化** 
        * 内置模块、自定义模块、第三方模块
        * require加载模块其他（非内置）时，会执行模块中的代码

    * **模块作用域**
        * 在自定义模块中定义的变量和方法等成员，只能在当前的模块中宝贝访问，这种模块级别的访问限制，就是模块作用域

    * **向外共享模块作用域中的成员**
        * `module`对象
            * 每一个js自定义模块中都有一个`module`对象，里面存储了和当前模块相关的信息
            * 通过`module.exports()`对象，可以向外共享
            * 导入模块的时得到的就是 `module.exports()`所指的对象（默认为空）
            * 向`module.exports()`对象添加属性
                * `module.exports.属性名 = 值/方法`示例`module.exports.sayHello = () => {console.log('Hello!')}`
            * node.js中提供了`exports`对象，默认情况下和`module.exports`指向同一个对象

```js
// 一开始exports和module.exports都指向存储username的对象
exports.username = 'zs'
// 为module.exports赋值新对象，但exports还是指向原来的对象
module.exports = {
	gender: '男',
	age: 22
}
// 用require导入包时，得到的一定是module.exports指向的对象

// 相比如果是添加属性，而不是赋值对象
module.exports.age = 20
// 那么得到的值既包含了exports添加的也包含了module.exports添加的属性
```

>导入结果永远以 `module.exports` 指向的对象为准

* Node.js中的模块规范
	* Node.js遵循了CommonJS模块化规范，规定了模块的特性和各模块之间如何相互依赖
	* CommonJS规定
		* 每个模块内部，`module`变量代表当前模块，`module`变量是一个属性，它的`exports`属性（module.exports）是对外接口，加载模块其实就是加载模块的`module.exports`属性(require()方法用于加载模块)

* ***npm与包***

    * 包
        * Node.js中的第三方模块也叫做包

        * 包是由第三方个人/团队开发，免费提供所有人使用
        * node.js包都是开源的

            [包搜索/文档](https://www.npmjs.com/) （直接浏览器）
            https://registry.npmjs.org/ （服务器下载地址，可自行在终端设置）

        * 版本号的语义化规范
            * 大版本.功能版本.bug修复版本
            * 当前面的版本 +1 ，后面的版本全部归零

        * 包管理配置文件
            * npm 规定，在**项目根目录**中，**必须**提供一个叫做 **package.json** 的包管理配置文件，用来记录与项目有关的一些配置信息
                * 项目的名称、版本号、描述等
                * 项目用到的包
                * 那些包只在开发期间用到
                * 那些包在开发和部署时都用到

            * 多人协作问题
                * 第三方包体积过大，不利于团队共享源代码，可以剔除项目的node_modules目录、将node_modules放到.gitignore忽略文件中

                * `npm init -y`快速创建 package.json文件

            * package.json
                * dependencies节点专门用于记录使用 `npm install` 安装的包，开发和上线都用到的
                * devDependencies节点只在项目开发阶段会用到，在项目之后不会用到，在安装包时添加 -D（--save-dev）参数代表记录到这个节点

            * 包的分类
                * 全局包
                    * 安装在node_global目录的包，全局可使用
                    * `npm install -g`
                * 项目包
                    * 安装到项目的node_modules目录的包
                    * 开发依赖包（devDependencies）`npm install -D`
                    * 核心依赖包（dependencies）`npm install`

* ***模块的加载机制***
    * 优先从缓存加载
        * 模块在第一次加载之后会被缓存，多次调用并不会导致模块代码多次被执行  
    * 内置模块加载机制
        * 内置模块是由官方提供，加载优先级别最高
    * 自定义模块的加载机制
        * 在加载自定义模块是必须指定`./或http://www.droliz.cn/`开头的路径标识符，负责会当作内置模块或第三方模块进行加载

        * 如果位置的扩展名，那么会按照以下顺序尝试执行
            * 按照确切文件名进行加载
            * 补全`.js`进行加载
            * 补全`.json`进行加载
            * 补全`.node`进行加载
            * 加载失败，终端报错

    * 第三方模块加载机制
        * 如果不是内置模块，也没有路径标识，那么就会从当前模块的父目录开始尝试从node_modules文件夹加载第三方模块，如果没有，向上冒泡，直到文件系统根目录 

    * 目录作为模块
        * 将目录作为模块标识符给require进行加载时
            * 查找package.json文件并寻找main属性作为加载的入口
            * 如果没有则加载index.js
            * 如果还没有在终端报错 `Error:Cannot find module 'xxx'`


### Express

* ***初识Express***

    [Express中文网站](http://www.expressjs.com.cn)

    * 基于Node.js平台，快速、开放、极简的 Web开发框架，与http模块类似，本质是Node.js的第三方包
    
    * 两种服务器
        * Web 网站服务器：专门对外提供 Web网页资源的服务器
        * API 接口服务器：专门对外提供API接口的服务器

    * 监听请求

```js
// 导入库
const express = require('express')
// 创建实例
const app = express();

// 监听请求
// GET
app.get('URL', function (res, req) {
	// res的send方法可以向客户端发送数据
	// 向客户端发生 JSON对象
	res.send({name: 'zs', age: 20, gender: '男'})
})

// POST
app.post('URL', function (res, req) {
	// 向客户端发送文本内容
	res.send('请求成功')
})

// 开启服务器
app.listen(端口号, callback)

// res.query对象存储url中查询字符串的参数，默认是空对象
// res.query = {name: 'zs', age: 20, gender: '男'}
// res.params通过:动态匹配URL的参数
// URL = http://www.droliz.cn.  :name
// res.params = {name: 'zs'}
```

* 托管静态资源
	* `express.static(fileName)`创建一个静态资源服务器 

```js
app.use([/路径前缀], express.static(对外开放的静态资源目录))
默认不带前缀，如果写了前缀参数，那么访问必须带前缀
```

* ***nodemon***

    * 编写调式node.js项目时，如果修改了项目代码，需要手动的close、重启，nodemon可以监听项目文件的改动，并自动重启项目

* ***Express路由***

    * 路由：客户端的请求与服务器处理函数之间的映射关系  

    * Express中的路由：请求的类型、请求的URL地址、处理函数

    * app.METHOD(PATH, HANDLERR) 

```js
app.get('/', function (req, res) {
	res.send('GET')
})

app.post('/', function (req, res) {
	res.send('POST')
})
```

* 客户端发送的请求会先经过路由的匹配，会**按照路由的顺序**进行匹配

* 路由模块化
	* 自定义一个路由的js文件，调用`express.Router`创建路由对象，向路由对象上挂载具体的路由，`module.exports`向外共享路由对象，使用`app.use()`注册路由模块

```js
// 路由模块.js
// 1、导入模块
const express = require('express')
// 2、创建路由对象
const router = express.Router()
// 3、挂载具体的路由
router.get('/path/list', (req, res) => {
	res.send('GET')
})
router.post('/path/list', (req, res) => {
	res.send('POST')
})
// 4、向外共享
module.exports = router

// js
// 导入路由模块
const userRouter = require('路由模块路径')
// 使用app.use()注册路由模块
app.use('/api', userRouter)

app.listen(3000, () =>{})
```

>`app.use()`用来注册**全局中间件**


* ***Express中间件***
    * 中间件：Middleware，特指业务流程的中间处理环节
    * 调用流程：当一个请求到达Express的服务器之后，可以连续调用多个中间件，对请求进行预处理
    ！[中间件的操作流程](http://www.droliz.cn/markdown_img/中间件的操作流程.jpg)

    * 格式：`app.METHOD('路径', (req, res, next) => {})`如果有next形参就是中间件处理函数

    *  next函数：表示把流程关系转交给下一个中间件或路由

```js
// 定义一个中间件函数
const mw = function(req, res, next) {
console.log('中间件函数');
// 业务逻辑处理完毕之后必须调用next函数
// 调用next表示将流转关系交给下一个路由或中间件
next();
}    
```

* 全局生效中间件：客户端发起的**任何请求**，到达服务器之后，都会触发的中间件
* 局部中间件：不适用app.use定义的中间件

* 中间件的作用
	* 多个中间件可以**共享同一份的req和res**，所以上游的中间件中统一为req或res添加自定义属性和方法供下游使用

```js
// 导入
const express = require('express');
const app = express();

// 全局中间件
app.use((req, res, next) => {
	console.log('全局中间件');
	// req(res).属性名(方法名) = 值   相当于全局变量，后面的中间件都可以使用
	next();
});

// 局部中间件（可以写多个用中括号包裹，执行从左往右）,只在当前路由生效，先给中间件处理，再给路由处理
app.get('/', (req, res, next) => {
	console.log('局部生效的中间件')
	next();     // 如果不写，无法将流程转换到后面的路由或中间件
}, (req, res) => {
	console.log('路由')
})

// 定义一个路由
app.get('/', (req, res) => {
	console.log('路由处理函数');
	res.send('Hello World');
});

// 开启服务
app.listen(3000, () => {
	console.log('服务已经开启 http://localhost:3000');
}
);

// 终端
// 全局中间件
// 局部生效的中间件  
// 路由
// 路由处理函数
```

* 中间件分类
* 应用级别中间件
	* 通过`app.use()/get()/post()`绑定到app实例上的中间件，叫做应用级中间件
* 路由级别中间件
	* 绑定到`express.Router()`实例上的中间件
* 错误级别中间件
	* 专门用来捕获项目中发生的异常错误，防止项目因为异常而崩溃`(err, req, res, next) => {}`，必须注册在所有路由之后（捕获前面所有错误）
* Express 内置中间件
	* `express.static`快速托管静态资源（无兼容性）
	* `express.json`解析JSON格式的请求数据体（仅在4.16.0之后）
	* `express.urlencoded`解析URL-encoded格式的请求体数据（仅在4.16.0之后）

```js
// 配置解析 application/json 格式的数据的内置中间件
app.use(express.json());
// 配置解析 application/x-www-form-urlencoded 格式数据的额内置中间件
app.use(express.urlencoded({extended: false}));
```

>当表单发送请求，如果不配置解析表单数据的中间件，req.body(请求体)默认为undefined
	
* 第三发放中间件
	* `npm install`下载、再导入由第三方开发的
	* `express.urlencoded`就是基于第三方`body-parser`开发的

* 自定义中间件

```javascript
const express = require('express');
const ps = require('querystring');
const app = express();

// 自定义模拟urlencoded的中间件
app.use((req, res, next) => {
	// 判断请求头是否为application/x-www-form-urlencoded
	if (req.headers['content-type'] === 'application/x-www-form-urlencoded') {
		// 解析请求体
		let body = '';
		// 监听data事件 （每次接收到数据都触发）
		req.on('data', (chunk) => {
			// 将数据拼接到body中
			body += chunk;
		});
		// 监听end事件 （接收完毕触发） 
		req.on('end', () => {
			// 使用querystring模块提供的parse方法将请求体转换为对象
			// 挂载为req.body，供后续中间件使用
			req.body = ps.parse(body);
			next();
		});
	} else {
		next();
	}
});
```

>`req.params`和`req.query`用在get请求中，`req.body`用在post请求中

* ***Express编写接口***

编写接口

```javascript
// 01.js(api)

const express = require('express');
const router = express.Router();

router.get('/get', (req, res) => {
	// 通过 req.query (get) 获取请求参数 (在post中用body)
	const query = req.query;
	// 通过 res.send 发送响应
	res.send({
		code: 200,
		msg: 'GET请求成功',
		data: query
	});
});

router.post('/post', (req, res) => {

	const body = req.body;
	// 通过 res.send 发送响应
	res.send({
		code: 200,
		msg: 'POST请求成功',
		data: body
	});
})

// 导出路由
module.exports = router;


// main.js
const cors = require('cors');
const express = require('express');
const app = express();
const api = require('./01.js');

// 配置解析表单数据的中间件(post请求)   
app.use(express.urlencoded({ extended: false }));

// 解决跨域
app.use(cors());

app.use('/api', api);

app.listen(3000, () => {
	console.log('server is running at port http://localhost:3000');
});

// post请求必须配置解析表单数据的中间件
```

 跨域问题
 
* 由于主要url的协议、域名、接口任意一项不相同，都有可能出现跨域的问题
* 解决跨域问题的两种方案
	* JSONP（只支持GET）
	* CORS（只支持XMLHTTPRequest Level2 浏览器，IE10+、chrome4+、FireFox3.5+）
		* CORS：（Cross-Origin Resource Sharing，跨域资源共享）由一系列**HTTP响应头**组成，决定了浏览器是否会阻止前端JS代码跨域获取资源
		* CORS是express的第三方中间件在路由之前通过`use`注册`cors()`即可

* CORS 响应头：
	* Access-Control-Allow-Origin
		* 响应头部可以携带一个 `Access-Control-Allow-Origin` 字段
			* `Access-Control-Allow-Origin: <origin> | *`
			* origin参数指定**允许访问该资源的外域URL**默认为通配符`*`表示所有的
			* `res.setHeader('Access-Control-Allow-Origin', 'https://www.Droliz.com')`只允许来自`http://www.droliz.com`的请求

	* Access-Control-Allow-Headers
		* 默认情况下只发送以下九个请求头，如果发送了额外的请求头信息，则需要在服务器端，通过Access-Control-Allow-Headers声明
			* `Accept`、`Accept-Language`、`Content-Language`、`DPR`、`Downlink`、`Save-Data`、`Viewport-Width`、`Width`、`Content-Type`（值仅限于`text/plain`、`multipart/form-data`、`application/x-www-form-urlencoded`三者之一）
		* `res.setHeader('Access-Control-Allow-Headers', 'Content-Type, X-Custom-Header')`

	* Access-Control-Allow-Methods
		* 默认cors仅支持 GET、POST、HEAD请求，可以通过Access-Control-Allow-Methods来指明请求所允许的HTTP方法

* CORS请求根据**请求方式和请求头**的不同，分为两大类
	* 简单请求
		* 请求方式是`GET、POST、HEAD`三者之一，请求头部是默认请求（都满足）
	* 预检请求
		* 除去默认的请求方式之外的、请求头包含自定义头部字段、向服务器发送了`application/json`格式的数据（任意一个）

>浏览器会先发送OPTION请求进行预检（预检请求），如果服务器允许该实际请求，并成功的响应了预检请求，才会发送真正的请求（携带真实数据）

* JSONP接口
	* JSONP不属于真正的Ajax请求，因为并没有使用XMLHttpRequest对象
	* JSONP仅支持GET请求，不支持POST、PUT、DELETE等请求

	* 如果项目配置了CORS跨域资源共享，为了防止冲突，必须在CORS之前声明JSONP接口，否则JSONP接口会被处理成开启了CORS的接口

```js
// 优先创建JSONP接口
app.get('/api/JSONP', (req, res) => { })
// 再配置CORS中间件（后面所有的接口都会被处理成CORS接口）
app.use(cors())
// 此接口在CORS之后，故开启了CORS
app.get('/api/get', (req, res) => { })
```

* JSONP接口的实现
	* 获取客户端发送的回调函数的名字、得到要通过JSONP形式发送给客户端的数据、结合两个拼接出一个函数调用的字符串、将字符串响应给`<script>`标签进行解析

```javascript
// JSONP
app.get('/api/jsonp', (req, res) => {
	// 获取客户端发送的回调函数名字
	const funcName = req.query.callback;
	// 得到通过 JSONP 形式发送给客户端的数据
	const data = { name: 'zs', age: 20 }
	// 拼接函数调用的字符串
	const scriptStr = `${funcName}(${JSON.stringify(data)})`;
	// 发送响应给客户端的 <script> 标签解析
	res.send(scriptStr);
});
```


## 拓展

### 跨域

**原生http**

1、直接在路由中输入

只能解决一部分简单的

```javascript
res.setHeader("Access-Control-Allow-Origin", "*");
```

**使用 express**

1、需要下载 `npm i cors -s` 中间件（推荐）

```javascript
app.use(cors());
```

2、请求头

更加全面

```javascript
app.all('*', function (req, res, next) {
	res.header("Access-Control-Allow-Origin", "*");
	res.header('Access-Control-Allow-Methods', 'PUT, GET, POST, DELETE, OPTIONS');
	res.header("Access-Control-Allow-Headers", "X-Requested-With");
	res.header('Access-Control-Allow-Headers', ['mytoken','Content-Type']);
	next();
});
```
