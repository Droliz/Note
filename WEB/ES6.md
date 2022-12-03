# ES6

## let变量声明

1、同 var 一样声明变量，不同的是let声明的变量不可以重复声明

```js
let a
let a = 1   // error
```

2、let声明的变量不仅仅具有全局作用域、函数作用域、eval作用域，还具有块级作用域

```js
{
	let a = 1
}
console.log(a)   // error
```


3、let 声明的变量不会存在变量提升

```js
console.log(a)
var a = 1

--------------------------

console.log(a)   // error
let a = 1   
```

4、不影响作用域

## const声明常量

1、必须赋初值

```js
const a    // error
const a = '' 
```

2、不可以被重新赋值

```js
const a = 1
a = 2    // error
```

注意这里对于数组对象的元素进行修改不算为修改常量

3、块级作用域


## 变量的解构赋值

ES6 允许按照一定的模式，从数组和对象中取值，然后赋值给变量

### 数组的解构

数组是按照顺序解构，可以使用`...`选择多个

```js
let [a, b] = [1, 2, 3, 4, 5]
console.log(a, b)  // 1, 2


let [a, ...b] = [1, 2, 3, 4, 5]
console.log(a, b)  // 1, [2, 3, 4, 5]
```

### 对象的解构

对象是按照属性和方法名解构

```js
let person = {
    name: "aaa",
    age: '12',
    func: function() {}
}
  
let {name, age, func} = person
```

## 模板字符串

ES6 采用模板字符串的方式 （反引号）

1、模板字符串中可以多行

```js
let str = `11
    222
    34321`
  
console.log(str);
```

2、直接使用变量

```js
let name = "111"

let str = `name: ${name}`
```

## 对象的简化写法

对象中可以直接写变量（属性名与变量同名）

```js
let name = "111"
  
let person = {
    name
}
```

函数也可以直接写在对象中

```js
let person = {
	talk() {// 函数体}
}
```

## 箭头函数

1、定义

```js
let func = (...agrs) => {// 函数体}
```

**2、this 是静态的，this始终指向函数声明时所在作用域下的this值（外层对象的this，自己是没有 this）**

```js
// html
function getName() {
	console.log(this.name)
}

const getName2 = () => {
	console.log(this.name);
}

window.name = "111"
const person = {
	name: "222"
}

getName()    // 111
getName2()   // 111

getName.call(person)   // 222
getName2.call(person)  // 111
```

3、不能作为构造函数去实例化对象

```js
let person = (name, age) => {
	this.name = name
	this.age = age
}

let me = new person("1", 18)
```

4、不能使用 `arguments` 变量（获取调用函数时的所有实参）

```js
let fun = () => {
	console.log(arguments);
}

fun(1, 2, 3)

// Error: Uncaught ReferenceError: arguments is not defined
```

5、只有一个参数可以省略小括号，当函数体只有一条语句可以省略花括号（return 也要省略）

```js
let a = b => b ** 2
```


## 函数参数默认值

ES6 允许给函数参数赋初始值，一般靠后

1、与解构赋值结合

```js
function connect({host='127.0.0.1', username, password}) {
	// code
}
```

## rest参数

ES6 引入 rest 参数，用于获取函数的实参，用来代替 arguments

1、rest 参数与 arguments

```js
function func() {
	console.log(arguments);   // 是一个对象
}

function func2(...args) {   // rest 参数
	console.log(args);   // 是一个数组
}

func("zs", 18, "男")
func2("zs", 18, "男")
```

**2、rest参数必须在参数列表的最后**

## 扩展运算符

`
`...`，扩展运算符可以将数组转换为逗号分割的参数序列

```js
const arr = [1, 2, 3, 4]

function func() {
	console.log(arguments)
}

func(...arr)
```

1、用于合并数组

```js
let arr1 = [1, 2, 3]
let arr2 = [...arr1, 4, 5]
```

2、合并对象

不同源对象的同名属性，后者覆盖前者

```js
let person = {
	name: "zs",
	age: 18
}

let lisi = {
	...person,
	name: "ls"
}
```

## Symbol数据类型

ES6 新增一种**原始数据类型**Symbol（随机值），表示独一无二（解决命名冲突问题），不能与其他数据类型进行运算。

Symbol定义的对象不能使用`for...in`遍历，但是可以使用`Reflect.ownKeys`获取所有键

### 声明

```js
let a = Symbol()

let s1 = Symbol('111')  // 描述字符串（注释作用）
let s2 = Symbol('111')  // 描述字符串（注释作用）
console.log(s1 === s2);  // false

// Symbol.for 创建
let s4 = Symbol.for('222')  // 这样创建的值是唯一的
let s5 = Symbol.for('222')  // 这样创建的值是唯一的

console.log(s4 === s5);   // true
```

### Symbol与对象

如果需要向一个对象中添加属性和方法，但是又不知道对象的结构（可能同名导致覆盖）

那么就可以添加Symbol类型

```js
let obj = {
	up: function() {},
	down: function() {},
	[Symbol('say')]: function() {}   // 直接写必须加 []
}
let methods = {
	up: Symbol('up'),
	down: Symbol('down')
}

obj[methods.up] = function() {}
obj[methods.down] = function() {}

console.log(obj);
```


```text
obj: 
1.  down: ƒ ()
2.  up: ƒ ()
3.  Symbol(down): ƒ ()
4.  Symbol(up): ƒ ()
```


**对象中直接写 Symbol 是不允许的，因为 `Symbol` 是一个动态值，必须加上 `[]`**

### Symbol 内置属性

#### Symbol.asyncIterator

符号指定了一个对象的默认异步迭代器。如果一个对象设置了这个属性，它就是异步可迭代对象

```js
const myAsyncIterable = new Object();
myAsyncIterable[Symbol.asyncIterator] = async function*() {
    yield "hello";
    yield "async";
    yield "iteration!";
};

(async () => {
    for await (const x of myAsyncIterable) {
        console.log(x);
        // expected output:
        //    "hello"
        //    "async"
        //    "iteration!"
    }
})();
```


#### Symbol.prototype.description

**`description`** 是一个只读属性，它会返回 `Symbol` 对象的可选描述的字符串。

```js
Symbol('desc').toString();   // "Symbol(desc)"
Symbol('desc').description;  // "desc"
Symbol('').description;      // ""
Symbol().description;        // undefined

// well-known symbols
Symbol.iterator.toString();  // "Symbol(Symbol.iterator)"
Symbol.iterator.description; // "Symbol.iterator"

// global symbols
Symbol.for('foo').toString();  // "Symbol(foo)"
Symbol.for('foo').description; // "foo"
```

#### Symbol.hasInstance

**`Symbol.hasInstance`用于判断某对象是否为某构造器的实例。因此你可以用它自定义 `instanceof`操作符在某个类上的行为。**

```js
class MyArray {
  static [Symbol.hasInstance](instance) {
    return Array.isArray(instance);
  }
}
console.log([] instanceof MyArray); // true
```

#### Symbol.isConcatSpreadable

内置的 **`Symbol.isConcatSpreadable`** 符号用于配置某对象作为`Array.prototype.concat()`方法的参数时是否展开其数组元素。

默认
```js
var alpha = ['a', 'b', 'c'],
    numeric = [1, 2, 3];

var alphaNumeric = alpha.concat(numeric);

console.log(alphaNumeric); // 结果：['a', 'b', 'c', 1, 2, 3]
```

设置为`false`
```js
var alpha = ['a', 'b', 'c'],
    numeric = [1, 2, 3];

numeric[Symbol.isConcatSpreadable] = false;
var alphaNumeric = alpha.concat(numeric);

console.log(alphaNumeric); // 结果：['a', 'b', 'c', [1, 2, 3] ]
```

#### Symbol.iterator

**Symbol.iterator** 为每一个对象定义了默认的迭代器。该迭代器可以被 `for...of`循环使用。

#### Symbol.match

**`Symbol.match`** 指定了匹配的是正则表达式而不是字符串。`String.prototype.match()` 方法会调用此函数。

#### Symbol.matchAll

**`Symbol.matchAll`** 返回一个迭代器，该迭代器根据字符串生成正则表达式的匹配项。此函数可以被 `String.prototype.matchAll()`方法调用。

#### Symbol.replace

**`Symbol.replace`** 这个属性指定了当一个字符串替换所匹配字符串时所调用的方法。`String.prototype.replace()` 方法会调用此方法。

#### Symbol.search

`Symbol.search` 指定了一个搜索方法，这个方法接受用户输入的正则表达式，返回该正则表达式在字符串中匹配到的下标，这个方法由以下的方法来调用 `String.prototype.search()`。

#### Symbol.species
知名的 **`Symbol.species`** 是个函数值属性，其被构造函数用以创建派生对象。

```js
class MyArray extends Array {
  // 覆盖 species 到父级的 Array 构造函数上
  static get [Symbol.species]() { return Array; }
}
var a = new MyArray(1,2,3);
var mapped = a.map(x => x * x);

console.log(mapped instanceof MyArray); // false
console.log(mapped instanceof Array);   // true
```

#### Symbol.split
**`Symbol.split`** 指向 一个正则表达式的索引处分割字符串的方法。这个方法通过 `String.prototype.split()`调用。

#### Symbol.toPrimitive

**`Symbol.toPrimitive`** 是内置的 symbol 属性，其指定了一种接受首选类型并返回对象原始值的表示的方法。它被所有的**强类型转换制算法**优先调用。

```js
// 一个没有提供 Symbol.toPrimitive 属性的对象，参与运算时的输出结果。
const obj1 = {};
console.log(+obj1); // NaN
console.log(`${obj1}`); // "[object Object]"
console.log(obj1 + ""); // "[object Object]"

// 接下面声明一个对象，手动赋予了 Symbol.toPrimitive 属性，再来查看输出结果。
const obj2 = {
  [Symbol.toPrimitive](hint) {
    if (hint === "number") {
      return 10;
    }
    if (hint === "string") {
      return "hello";
    }
    return true;
  },
};
console.log(+obj2); // 10  — hint 参数值是 "number"
console.log(`${obj2}`); // "hello"   — hint 参数值是 "string"
console.log(obj2 + ""); // "true"    — hint 参数值是 "default"
```

#### Symbol.toStringTag
**`Symbol.toStringTag`** 是一个内置 symbol，它通常作为对象的属性键使用，对应的属性值应该为字符串类型，这个字符串用来表示该对象的自定义类型标签，通常只有内置的 `Object.prototype.toString()`方法会去读取这个标签并把它包含在自己的返回值里。

#### Symbol.unscopables
**`Symbol.unscopables`** 指用于指定对象值，其对象自身和继承的从关联对象的 with 环境绑定中排除的属性名称。


## 迭代器

一种接口，为了配合 ES6 推出的 `for...of` 使用。`Array、Arguments、Set、Map、String、TypedArray、NodeList`都可以采用 `for...of` 遍历（只要有`Symbol.iterator`那么就可以使用`for...of`）

原理：创建一个指针对象，指向起始位置，每次调用 `next` 方法就会向后移动（第一次指向第一个），每次调用会返回一个包含`value`和`done`的对象

```js
const arr = [1, 2, 3, 4, 5, 6]

for (let i of arr) {
	console.log(i);
}
```

`for...of` 获取的是值，`for...in` 获取的是键

一般需要自定义遍历数据时使用迭代器（重写`Symbol.iterator`）

```js
const obj = {
	name: "zs",
	age: 18,
	friends: [
		'xm',
		'ls',
		'ww',
		'zl'
	],
	[Symbol.iterator]() {   // 自定义
		let index = 0
		return {
			next: () => {
				if (index < this.friends.length) {
					const res = {
						value: this.friends[index],
						done: false
					}
					index ++
					return res
				} else {
					return {
						value: undefined,
						done: true
					}
				}
			}
		}
	}
}


// 自定义遍历 friends
for (let i of obj) {
	console.log(i);
}
```


## 代理对象proxy

代理对象可以对一个普通对象的操作进行拦截，然后自定义操作

```js
const obj = {}

// 代理对象
const objProxy = new Proxy(obj, {
	get() {},
	set() {}
})
```

代理对象中有13中拦截方法

**1、get(target, propKey, receiver)：**

拦截对象属性的读取，比如`proxy.foo`和`proxy['foo']`。

**2、set(target, propKey, value, receiver)：**  
拦截对象属性的设置，比如`proxy.foo = v`或`proxy['foo'] = v`，返回一个布尔值。

**3、has(target, propKey)：**

拦截`propKey in proxy`的操作，返回一个布尔值。

**4、deleteProperty(target, propKey)：**

拦截`delete proxy[propKey]`的操作，返回一个布尔值。

**5、ownKeys(target)：**

拦截`Object.getOwnPropertyNames(proxy)`、`Object.getOwnPropertySymbols(proxy)`、`Object.keys(proxy)`、`for...in`循环，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而`Object.keys()`的返回结果仅包括目标对象自身的可遍历属性。

**6、getOwnPropertyDescriptor(target, propKey)：**

拦截`Object.getOwnPropertyDescriptor(proxy, propKey)`，返回属性的描述对象。

**7、defineProperty(target, propKey, propDesc)：**

拦截`Object.defineProperty(proxy, propKey, propDesc）`、`Object.defineProperties(proxy, propDescs)`，返回一个布尔值。

**8、preventExtensions(target)：**

拦截`Object.preventExtensions(proxy)`，返回一个布尔值。

**9、getPrototypeOf(target)：**

拦截`Object.getPrototypeOf(proxy)`，返回一个对象。

**10、isExtensible(target)：**

拦截`Object.isExtensible(proxy)`，返回一个布尔值。

**11、setPrototypeOf(target, proto)：**

拦截`Object.setPrototypeOf(proxy, proto)`，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截。

**12、apply(target, object, args)：**

拦截`Proxy` 实例作为函数调用的操作，比如`proxy(...args)`、`proxy.call(object, ...args)`、`proxy.apply(...)`。

**13、construct(target, args)：**

拦截 `Proxy` 实例作为构造函数调用的操作，比如`new proxy(...args)`。

## 生成器

### 生成器是什么

生成器是ES6中新增的一种函数控制、使用的方案，它可以让我们更加灵活的控制函数什么时候继续执行、暂停执行等。平时我们会编写很多的函数，这些函数终止的条件通常是返回值或者发生了异常。

生成器函数也是一个函数，但是和普通的函数有一些区别：

- 首先，生成器函数需要在function的后面加一个符号：`*`
- 其次，生成器函数可以通过yield关键字来控制函数的执行流程：
- 最后，生成器函数的返回值是一个 `Generator`（生成器）：
	- 生成器事实上是一种特殊的迭代器；

### 简单使用

```js
function * gen() {
	console.log('第一个代码块');
	yield '第一个结束'
	console.log('第二个代码块');
	yield '第二个结束'
}

let iterator = gen()

console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
```


```text
第一个代码块
{value: "第一个结束", done: false}
第二个代码块
{value: "第二个结束", done: false}
{value: undefined, done: true}
```

`yield` 相当于代码块的分割，只有调用`next`时才会执行这一部分代码块，并返回一个对象包含`value`和`done`

同样的可以采用`for...of`遍历

```js
for (let i of iterator) {
	console.log(i)  
}
```


### 函数参数

调用`next`时是可以传入参数的，此时传入的参数就作为上一次next方法的返回值

```js
function* gen() {
	let B = yield '111'
	console.log(B);   // BBB
	let C = yield '222
	console.log(C);   // CCC
	let D = yield '333'
	console.log(D);   // DDD
}

let iterator = gen()

console.log(iterator.next());
console.log(iterator.next('BBB'));
console.log(iterator.next('CCC'));
console.log(iterator.next('DDD'));
```


### 实例

定时器回调

```js
setTimeout(() => {
    console.log(111);
    setTimeout(() => {
        console.log(222)
        setTimeout(() => {
            console.log(333)
        }, 3000)
    }, 2000)
}, 1000)
```

使用生成器

```js
const one = () => {
    setTimeout(() => {
        console.log(111);
        it.next()
    }, 1000)
}
const two = () => {
    setTimeout(() => {
        console.log(222);
        it.next()
    }, 2000)
}
const three = () => {
    setTimeout(() => {
        console.log(333);
        it.next()
    }, 3000)
}
  
function * gen() {
    yield one()
    yield two()
    yield three()

}
  
let it = gen()
it.next()
```


获取数据（按顺序的）

```js
function getUser() {
    // 发起请求获取后端数据 .....
    setTimeout(() => {
        let data = '用户数据'
        iterator.next(data)  // 第二次调用
    }, 1000)
}
function getOrder(user) {
  
    console.log(`获取到数据user ${user} 开始请求`);

    // 发起请求获取后端数据 .....
    setTimeout(() => {
        let data = '订单数据'
        iterator.next(data)   // 第三次调用
    }, 1000)
}
function getGood(order) {
  
    console.log(`获取到数据order ${order} 开始请求`);
  
    // 发起请求获取后端数据 .....
    setTimeout(() => {
        let data = '商品数据'
        iterator.next(data)   // 第四次调用
    }, 1000)
}
  
function * gen() {
    let user = yield getUser()
    let order = yield getOrder(user)
    let good = yield getGood(order)
  
    console.log(good);
}
  
let iterator = gen()
iterator.next()   // 第一次调用
```

只有获取了前面的数据才能获取后面的数据，此时这些数据可以作为参数传递给后面的函数调用


## Promise

ES6 解决异步编程的新方案，语法上Promise是一个构造函数，用于封装异步操作并获取成功和失败的结果

### 实例化

接收一个函数，函数接收两个参数`resolve, reject`（默认）

实例对象有三个状态：初始化、成功、失败（只会改变一次）

```js
const p = new Promise((resolve, reject) => {
    setTimeout(() => {
        let data = "成功获取到数据"
        resolve(data)    // resolve 会将 p 对象改为成功状态
        let err = "获取数据失败"
        reject(err)   // 更改为失败状态
    }, 1000)
})
```


### then方法

`then`方法接收两个函数，返回结果也是一个 `Promise`，此`promise`的状态由回调函数执行结果决定

如果回调返回是一个非 `Promise` 数据那么状态为成功，且返回值为对象的成功值。如果返回值是`Promise`那么由内部的`promise`状态决定

当状态为成功时，`p.then`会调用第一个回调函数，状态为失败时会调用第二个回调

```js
p.then((value) => {
	// 成功回调
}, (reason) => {
	// 失败回调
})
```

### catch方法

指定 `Promise` 对象失败的回调

```js
p.catch(err => {
	// 失败回调
})
```


### 封装读取文件

```js
function getFile(path) {
    return new Promise((resolve, reject) => {
        fs.readFile(path, (err, res) => {
            if (err) reject(err)
            resolve(res)
        })
    })
}

// 返回的是 Promise 对象，所以需要使用 .then 处理
```

读取多个文件

```js
const p1 = getFile("F:\\vs_code\\web\\JavaScript\\markdown\\HTML.md")
const p2 = getFile("F:\\vs_code\\web\\JavaScript\\markdown\\CSS.md")
const p3 = getFile("F:\\vs_code\\web\\JavaScript\\markdown\\JS.md")
  
p1.then(value => {
    console.log(value.toString().slice(0, 110));
    return p2   // 返回 Promise
}).then(value => {
    console.log(value.toString().slice(0, 110));
    return p3
}).then(value => {
    console.log(value.toString().slice(0, 110));
})
```


## Set

ES6 提供的新的数据结构 Set，实现了`iterator`接口，可以使用 `for...of` 遍历

### 基本使用

```js
let s = new Set()
  
s.add(1)
s.add(2)
s.add(3)
s.add(4)
s.add(5)
let a = s.add(1)   // 已经存在不生效会更改原始set且会返回set
s.delete(5)  // 删除
  
console.log(s.has(5));
console.log(s.size)   // 个数
console.log(s);
  
s.clear()  // 清空

for (let i of s) {
	consle.log(i)
}
```


### 交并差

```js
let arr1 = new Set([1, 2, 3, 4, 5])
let arr2 = new Set([2, 3, 4, 10, 11])
  
// 交集
const res = [...arr1].filter(item => arr2.has(item))
  
// 并集
const union = [... new Set([...arr1, ...arr2])]
  
// 差集
const diff = [...arr1].filter(item => !arr2.has(item))

console.log(res);
console.log(union);
console.log(diff);
```

## Map

以键值对的形式存储数据，实现了`iterator`接口，可以使用 `for...of` 遍历

## class类

使用`class`声明，`constructor`定义构造函数初始化，`extends`继承父类，`super`调用父类方法，`static`定义静态方法，父类方法可以重写

```js
class Person {
    constructor(name, age) {
        this.name = name
        this.age = age
    }
  
    func() {
        console.log("func");
    }
}
```

静态成员只能由类访问，实例对象无法访问

子类调用父类构造方法`super(参数...)`

### getter、setter

在调用属性时会调用get方法，更改属性值时会调用set方法

```js
class Person {
    name
    age
    get name() {
        return this.name
    }
  
    set name(value) {
        this.name = value
    }
  
    get age() {
        return this.age
    }
  
    set age(value) {
        this.age = value
    }
}

  
const a = new Person()
console.log(a);
a.name = 'zs'
a.age = 18
console.log(a);
```


## 数值扩展

### Number.EPSILON

`Number.EPSILON` 是JS表示的最小精度

所以在判断是否相等时，可以通过判断差值与`Number.EPSILON`的大小来判断是否相等的

```js
// 解决浮点数运算精度问题
function equal(a, b) {
    return Math.abs(a - b) < Number.EPSILON
}
  
console.log(0.1 + 0.2 === 0.3);   // false
console.log(equal(0.1 + 0.2, 0.3));   // true
```


### 二进制和八进制

`0b`开头代表二进制、`0o`开头代表八进制、`0x`开头代表十六进制

```js
let a = 0b1010
let b = 0o1276
let c = 0x1fe
```

### Number.isFinite

判断数值是否为有限数

```js
console.log(Number.isFinite(100))   // true
console.log(Number.isFinite(100/0))    // false
console.log(Number.isFinite(Infinity))   // false
```

### Number.isNaN

判断是否是 `NAN`

```js
console.log(Number.isNaN("1" + 2))   // true
console.log(NUmber.isNaN(1))  // false
```

### Number.parseInt、Number.parseFloat

字符串转整型和浮点型

```js
console.log(Number.parseInt("13aaa"));  // 13
console.log(Number.parseFloat("13.90aaa"));   // 13.9
```

### Number.isInteger

判断是否为整型，如果是`19.0`那么会判断为是整数

```js
console.log(Number.isInteger(19));   // true
console.log(Number.isInteger(19.5));  // false
```

### Math.trunc

向下取整

```js
console.log(Math.trunc(19.4));  // 19
console.log(Math.trunc(19.6));  // 19
```

### Math.sign

判断数值正负值和零

```js
console.log(Math.sign(2));   // 1
console.log(Math.sign(0));   // 0
console.log(Math.sign(-2));  // -1
```


## 对象扩展方法

### Object.is

判断两个值是否完全相等，类似于全等，不同的是判断`NaN`为`true`

```js
console.log(Object.is(1, 1));   // true
console.log(Object.is(NaN, NaN));   // false
console.log(NaN === NaN);   // true
```

### Object.assign

对象合并，重名的属性和方法，后面的会覆盖前面的

### Object.setPrototypeOf、getPrototypeOf

设置和获取原型对象

```js
const obj = {name: "obj"}
  
const obj1 = { name: "obj1"}
  
Object.setPrototypeOf(obj, obj1)   // 设置 obj 原型对象为 obj1
console.log(Object.getPrototypeOf(obj))    // {name: "obj1"}
console.log(obj);  // {name: "obj"}
```


## 模块化

`export`向外暴露数据，`import` 导入

### 默认导出导入

`export default 默认导出成员`（全文件只能写一次）

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

有两种写法

```js
// 第一种多次暴露
export const a = ""
export const b = function () {}


// 统一暴露
export {
	a,
	b
}
```

`import { 成员1 as 别名, 成员2, 成员3 …… } from 模块`这里的成员名称必须和导出的名称一致，可以使用as重命名

### 直接导入并执行模块代码

`import 模块标识符`会直接执行模块的代码
