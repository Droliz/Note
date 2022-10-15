# JavaScript

---

##  JavaScript 的认识

### 简介

JavaScript是一种运行在客户端的脚本语言

脚本语言：不需要编译，运行过程由js解析器（js引擎）逐行进行解释并执行。现在也可以基于Node.js技术进行服务器端编程

### 作用

* 表单动态检验（密码强度检测）
* 网页特效
* 服务端开发（Node.js）
* 桌面程序（Election）
* APP（Cordova）
* 控制硬件-互联网（Ruff）
* 游戏开发（cocos2d.js）

### HTML、CSS、JavaScript关系

HTML决定网页的结构和内容

JavaScript实现业务逻辑和页面控制

HTML、CSS是描述类语言，JS是编程类语言

### 浏览器执行JS
* 浏览器分为两个部分：渲染引擎、JS引擎
	* 渲染引擎：用于解析HTML、CSS，俗称内核.如chrome浏览器的blink、老版本的webkit
	* JS引擎：也称为JS解释器，用来读取网页中的JavaScript代码，对齐处理后运行.如：chrome浏览器的V8

>浏览器本身是不会执行JS代码，而是通过内置的JavaScript引擎来执行JS代码，JS引擎执行代码时逐行解释每一句代码（转换为机器语言），然后交由计算机执行，所有JavaScript语言归为脚本语言，会逐行解析代码

### JS组成
* ECMAScript是由ECMA国际进行的标准化的一门编程语言，JavaScript和JScript是ECMAScript的实现和扩展
	* ***ECMAScript规定了JS的编程语法和核心知识***
* DOM（页面文档对象模型）
	* DOM是由3C组织推荐的处理可扩展标记语言的标准编程接口，可以对页面上的元素进行操作
* BOM（浏览器对象模型）
	* BOM提供了独立于内容的、可以与浏览器窗口进行互动的对象结构，通过BOM可以操作浏览器窗口.如：弹窗、控制浏览器跳转、获取分辨率等

### JS书写位置
* 内嵌式
	* 包裹在`<script>JS语句</script>`中一般在`<title>`标签下，同CSS

* 行内式
	* 直接写在元素的属性内部（以on开头的属性）

* 外联式
	* 写在js文件中，通过`<script scr="JS文件"></script>`引入，引入时，标签中不能写任何代码  

>在script标签中，有一个type属性，默认值为text/javascript代表这个包裹的代码当做js解析

---

## JS基本语法

### 注释

* //：单行注释
* /* \*/：多行注释

### 输入输出

|       方法       |              说明              |  归属  |
| :--------------: | :----------------------------: | :----: |
|    alert(msg)    |        浏览器弹出警示窗        | 浏览器 |
| console.log(msg) |    浏览器控制台打印输出内容    | 浏览器 |
|   prompt(info)   | 浏览器弹出输入框，用户可以输入 | 浏览器 |

>输入的数据是字符型

### 变量

* 变量是程序在内存中申请的一块用来存放数据的空间
* 变量命名：var 变量名;（在程序运行过程中，数据类型会被自动确定）
* 声明新变量时，也可以使用关键词 "new" 来声明其类型：`var 变量名 = new 变量类型`

### 数据类型
JS中数据的类型是可以变化的

简单数据类型：Number、String、Boolean、Undefined(声明变量但未赋值)、Null

复杂数据类型：object

#### 数字型

进制： 
八进制：0 ~ 7 在数字前面加 0   
十六进制：0 ~ 9 a ~ f 在数字前面加 0x
二进制：0 和 1组合

数字型范围
* 最大：Number.MAX_VALUE
* 最小：Number.MIN_VALUE

数字型三个特殊值
* 无穷：Infinity（大）、-Infinity（小）
* NaN：非数字

>可以用isNaN判断是否是非数字，返回布尔值

#### 字符串

* 变量名.length：获取字符串长度
* 字符串 + 任意类型 = 拼接后的新字符串
* typeof 变量名：获取变量数据类型

#### 数据类型转换
* 转为字符串

|         方式         |             示例              |
| :------------------: | :---------------------------: |
|       toString       | `var num = 1; num.toString()` |
|  String（强制转换）  |  `var num = 1; String(num)`   |
| ***加号拼接字符串*** | `var num = 1; num = num + ""` |

* 转为数字型

| 方式                 | 说明                         | 案例                |
| :------------------- | :--------------------------- | :------------------ |
| parseInt(String)     | 将String转换为整数数值型     | `parseInt('78')`    |
| parseFloat(String)   | 将String转换为浮点数数值型   | `parseFloat('7.8')` |
| Number()强制转换函数 | 将String转换为数值型         | `Number('12')`      |
| js隐式转换（. * /）  | 利用算术运算隐式转换为数值型 | '12'.0              |

* 转为布尔

| 方式      | 示例            |
| :-------- | :-------------- |
| Boolean() | Boolean("true") |

#### 运算符
* 算术运算符：+、-、*、/、%
* 递增递减运算符：
	* ++i：先自增后返回
	* i++：先返回后自增
* 比较运算符：<、>、\==、>=、<=、\=\==、!=、!==
	* \=\==、!\==：全等，要求 ***值和数据*** 都相同
* 逻辑运算符：&&、||、!
	* 短路运算：当多个表达式时前面的表达式已经可以决定结果时，不用继续计算
		* 逻辑与
		* 表达式1 && 表达式2
		* 表达式1为true返回表达式2，否则返回表达式1
		---
		* 逻辑或
		* 表达式1 || 表达式2
		* 表达式1为true返回表达式1，否则返回表达式2

* 赋值运算符
	* 变量 = 表达式;、变量 += 表达式; 等
	* 将表达式的值赋给变量
	* ***如果将值赋给未定义的变量时，将会被自动作为widows的一个属性***
	* 没有声明的变量可配置全局属性，可以删除

### 流程控制
#### if-else

```js
if (判断条件) {
	执行语句
}
else if (判断条件) {
	执行语句
}
else {
	执行语句
}
```

三元表达式
* 条件表达式 ? 表达式1 : 表达式2
* 如果条件表达式为true则返回表达式1的值，否则返回表达式2的值

#### switch

```js
switch (表达式) {
	case value1: 
		执行语句1;
		break;
	case value2: 
		执行语句2;
		break;
	default:
		执行最后语句;
		break;
}
```
    
#### for —— for/in

```js
for (语句1; 语句2; 语句3) {
	执行语句;
}
```

* 语句1：循环开始前执行
* 语句2：循环条件
* 语句3：每次循环后执行

```js
for (属性名 in 可遍历对象) {
	执行语句;
}
``` 

#### while —— do-while

```js
while (条件语句) {
	执行语句;
}
```

* 先判断条件后执行语句

```js
do {
	执行语句;
}
while (条件语句);
```

* 先执行语句后判断条件，至少执行一次

* **break —— continue**
* break会跳出整个循环，执行循环下面的代码
* continue会跳出此次循环，执行下一次循环

### 对象

#### 自定义对象

* var 对象名 = new object();
* 对象是键值对的容器{name: value}，键值对在JS中通常称为对象属性
* 添加属性：对象名.属性名 = 属性值
* 添加方法：对象名.方法名 = function() {代码块}

```js
var person = {name: 'Ail',
	age: 28,
	ph: '123456789',
	sex: '男',
	函数名: function() {代码块}
}
```

#### 内置对象
* JS自带的对象，可以直接使用实现一些效果
	* 例如：数组对象
	* `var 数组名 = new Array();`

| 方法名                                 | 说明                                               |
| :------------------------------------- | :------------------------------------------------- |
| push()                                 | 在数组末尾添加数据，并返回新长度                   |
| pop()                                  | 删除末尾元素，并返回被删除的元素                   |
| unshift()                              | 在数组前面添加数据，并返回新长度                   |
| shift()                                | 删除第一个元素，并返回被删除的元素                 |
| splice(索引,要删除元素个数,添加的数据) | 在数组指定位置添加数据                             |
| sort(function(a, b){return 值})        | 值 = a - b（升序）值 = b - a（降序），返回新数组   |
| reverse()                              | 翻转数组，返回新数组                               |
| indexOf()                              | 从前往后查找返回给定元素第一个索引，如果没有返回-1 |
| LastIndexOf()                          | 从后往前，同indexOf                                |
| toString()                             | 将数组转换为字符串逗号隔开，返回字符串             |
| join(分隔)                             | 同toString默认逗号，可填分隔                       |

#### 浏览器对象

### 函数
#### 函数使用

```js
// 声明函数方法1
function 函数名(参数) {
	代码块 

	return 数值; // 函数返回值(可选)
}

// 声明函数方法2
var 变量名 = function() {代码块}

// JS中调用函数
函数名(参数);

// 行内式调用函数
<button onclick="函数名(参数)"></button>  
```

#### 构造函数
* 构造函数首字母大小，不需要return就可以返回一个对象
* 将对象中相同的 ***属性和方法*** 封装到函数中

```js
// 构造函数
function 构造函数名() {
	this.属性名 = 属性值;
	this.方法名 = function() {代码块}
}
// 调用构造函数
new 构造函数名();
```

示例:

```js
function Start(uname, age, sex) {
	this.uname = uname;
	this.age = age;
	this.sex = sex;
	this.sing = function(sang) {
		console.log(sang);
	}
}
var ldh = new Start('刘德华', 18, '男');
ldh.sing('唱歌!');
console.log(ldh.uname);
``` 

>new关键字执行过程：先在内存中创建一个空对象
>this就会指向这个空对象
>执行代码构造函数代码然后给这个空对象添加属性、方法
>最后返回这个对象

在不确定有多少实参传入时，arguments存储了所有的实参，值按传入顺序 (数组)

```js
// 构造函数  
function Person() {  
  
}  
  
// 实例  
let person1 = new Person()  
person1.name = "jack"  
console.log(person1.name)   // jack
```

原型对象就相当于一个公共的区域，所有同一个类的实例都可以访问到这个原型对象，我们可以将对象中共有的内容，统一设置到原型对象中在JavaScript中。每定义一个函数数据类型会自带一个 `prototype` 属性（显示原型属性，只有函数有），这个属性是一个指针指向==函数的原型对象== `Person.prototype`

**构造函数与原型之间的关系**

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220903182122.png)


```js
Person.prototype.name = "jack"  
let person1 = new Person()  
let person2 = new Person()  
console.log(person1.name)   // jack  
console.log(person2.name)   // jack
```

在实例对象上，有一个`__proto__` 属性（隐式原型属性）是指向原型对象

```js
let person1 = new Person()  
console.log(person1.__proto__ === Person.prototype)   // true
```


![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220903183307.png)


原型对象上有一个 `constructor` 属性，这个属性就是将原型对象指向关联的构造函数

```js
console.log(Person.prototype.constructor === Person)  // true

console.log(person1.constructor === Person)   // true
```

上述中，实例对象上并没有`constructor` 属性，但是会通过原型链在原型（`Person.prototype`）上找到

当实例和原型有一个相同的属性时，输出实例的属性会显示实例上属性，自动忽略原型属性。访问实例属性会先在实例上查找该属性，如果没有就在原型上查找，如果没有找到原型的原型，直到最后没找到就为 `null`

实例`hasOwnProperty` 方法是无法检测是否具有原型上定义的属性，直接使用 `in` 操作符是可以检测所有的

```js
Person.prototype.name = '1'  
let person1 = new Person()  
person1.age = 1  
console.log(person1.hasOwnProperty('name'))  // false  
console.log(person1.hasOwnProperty('age'))  // true  
console.log("name" in person1)  // true  
console.log("age" in person1)  // true
```

需要知道的是，通过对象的`keys` 方法获取可枚举的属性名，其中并没有 `name`，也就是说实例身上是可以访问`name`，但是该属性是不可枚举的

```js
let keys = Object.keys(person1)  
console.log(keys)   // [ 'age' ]
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220903185219.png)



> ***变量、属性、函数、方法的区别***
>变量和属性都是用来存储数据的，变量单独声明，使用时直接写变量名.属性在对象中，不需要声明，使用时必须是对象名.属性名
>函数和方法都是实现某种功能，函数单独定义，调用时 函数名(参数);.方法在对象中，调用的时候 对象名.方法名(参数)

* **作用域**
    * 可访问的变量、函数、对象的集合
    * 变量在函数内声明，变量为局部作用域只能在函数内部访问，因为是局部变量，所有不同的函数可以有相同的变量名，局部变量在函数执行结束后就会被销毁
    * 变量在函数外声明或在函数内没有声明（没有var关键字），变量为全局作用域，全局变量只能有一个相同名字，在网页关闭的时候销毁

## BOM和DOM

[JS对象查阅手册](https://www.runoob.com/jsref/jsref-tutorial.html)


## API和WebAPI
* API
    * API是一些预先定义的函数工具，以便更轻松的实现想要完成的功能
* WebAPI
    * 是浏览器提供的一些操作浏览器功能和页面元素的API（BOM、DOM）


## 注意

浏览器总是习惯性的打印出你执行函数后的返回值，所以在有些情况下使用`console.log()` 打印会在末尾打印一个`undefined`