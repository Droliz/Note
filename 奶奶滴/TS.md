# TypeScript

## 简介

### TS与JS区别

TS 是 JS 的超集（JS 有的 TS 都有）

~~~ts
TypeScript = Type + JS (在 JS 基础上，为 JS 添加了类型支持)

// TS 声明变量
let age: number = 28
// JS 声明变量
let age = 18
~~~

TS 属于静态类型的编程语言，JS 属于动态类型的编程语言。对于 JS 等到代码执行时才能发现错误，而 TS 在编译时就可以发现错误。添加的类型支持很大一部分解决了 JS 中的类型错误（绝大多数）

>静态类型编程语言：在编译的时候做类型检查
>动态类型编程语言：在执行时做类型检查

### TS 优势

* 1、更早发现错误，提高开发效率
* 2、程序中任何位置都有代码提示
* 3、强大的类型系统提升了代码的可维护性，使重构代码更加容易
* 4、支持最新的 `ECMAScript` 语法
* 5、TS 类型推断机制，不需要在每个地方都标注类型
* 6、Vue 3 源码使用 TS 重写、Angular 默认支持 TS、React 与 TS完美配合

### 安装编译 TS 工具包

node.js 和浏览器只认识 JS 代码，不认识 TS 代码，所以需要先使用工具包将 TS 代码转为 JS 代码才可以被浏览器和 node.js 识别

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220624060003.png)
全局安装typescript工具包

~~~sh
npm i -g typescript
~~~

`typescript` 包提供了 `tsc` 命令，实现 `TS -> JS` 的转化

```sh
# 查看 typescript 版本
tsc -v

# 编译 ts 文件（自动生成一个同名的 JS 文件）
tsc 文件名.ts

# 运行 js 文件
node 文件名.js
```

***注意：由 TS 编译生成的 JS 文件中，没有类型信息***

使用 `ts-node` 包简化运行 TS 

安装 `ts-node` 依赖

~~~sh
npm i ts-node -g
~~~

运行 ts 代码

~~~sh
# 简化了运行步骤（不会直接生成编译后的 JS 文件）
ts-node 文件名.ts
~~~

## TS 常用类型
### TS 类型检测

JS有类型，但是不会检查变量的类型是否发生变化，ts会检查类型的变化，可以显示标记出代码中的意外行为

~~~TS
// ts
var age: number = 20;
age = '20'    // 编码是报错：不能将 String 分配给类型 Number
age.toFixed()

// js
let count = 18
count = '20'
count.toFixed()   // 字符串没有 toFixed 方法，但是在编码时不会报错
~~~

### 类型注解

在如下的代码中，`: number` 就是类型注解

作用：为变量添加类型约束，约束给该变量赋值的值类型

~~~ts
let age: number;
~~~

### 常用基础类型

#### TS 与 JS 

* JS已有类型
	* 基本类型：number/string/boolean/null/undefined/symbol
	* 对象类型：object（数组、对象、函数等）
* TS新增类型
	* 联合类型、自定义类型（类型别名）、接口、元组、字面量类型、美剧、void、any等

#### 基本类型

ts 的基本类型和 js 的相同，在声明类型时，ts需要带类型注解

~~~ts
let age: number = 18;
let myName: string = '张三';
let isLoading: boolean = false;
let a: null = null;
let b: undefined = undefined;
let c: symbol = Symbol();
~~~


#### 数组

数组类型的两种声明（推荐使用 `类型[]` 方式）

~~~ts
let numbers: number[] = [1, 2, 3, 4, 5, 6, 7, 8]
let strings: Array<string> = ['a', 'b', 'c']
~~~

#### 联合类型

如果需要数组中有多个不同的类型，则可以用到联合类型

~~~ts
let arr: (number | string)[] = [1, 2, 'a', 'b']
// | 在ts中叫做联合类型（有两个或两个以上的不同类型组成）
~~~

#### 类型别名

类型别名（自定义类型）：为任意类型起别名

当一个类型比较复杂，起类型的别名，简化类型的使用

* 使用 `type` 关键字创建类型别名、
* 创建类型别名后，可以直接将类型别名当类型注解使用

```ts
type CustomArray = (number | string)[];
let arr: CustomArray;
```

#### 函数类型

函数类型：函数的参数和返回值类型

为函数指定类型
* 单独指定参数、返回值类型
	```ts
	const add = (num1: number, num2: number): number => {
	    return num1 + num2
	}
	  
	function add2(num1: number, num2: number): number {
	    return num1 + num2
	}
	```
* 同时指定参数、返回值类型
	```ts
	const add: (num1: number, num2: number) => number = (num1 , num2) => {
	    return num1 + num2;
	}


	// 也可以使用类型别名
	type func = (num1: number, num2: number) => number;
	  
	const add: func = (num1 , num2) => {
	    return num1 + num2;
	}
	```
	当函数转为表达式时，可以通过类似箭头函数的形式为函数添加类型

返回类型为 `void` 时代表无返回值

**函数可选参数**

在参数后面添加 `?` 代表此参数为可选参数（可选参数必须放在参数列表的最后）

~~~ts
function add(num1?: number, num2?: number): number {
	return num1 + num2
}

function add(num1: number, num2?: number): number {
	return num1 + num2
}
~~~


#### 对象类型

JS 中的对象是由属性和方法构成的，而 TS 中的对象的类型就是在描述对象的结构

```ts
// 也可以用 ; 隔开
let person: {   
    name: string,   
    age?: number,   // 对象的可选属性 采用 ?: 形式
    sayHi(): void 
};

peroson = {  
    name: 'jack',  
    age: 18,  
    sayHi() {}  
}

// 方法也可以采用箭头函数的形式
sayHi: () => void
```


#### 接口类型

当一个对象类型被多次使用，一般会用接口（ `interface` 关键字）来描述对象的类型

```ts
// 使用 interface 关键字声明接口
interface person {
    name: string,
    age: number,
    sayHi(): void
};

/* 使用类型别名
type person = {
    name: string,
    age: number,
    sayHi(): void
};
*/

// 定义的接口名称即为变量的类型（与自定义类型不同）
let jack: person = {
    name: 'Jack',
    age: 18,
    sayHi: function () {
        console.log('Hi, I am Jack');
    }
};

console.log(jack)
```

**与类型别名区别**

接口只能为对象指定类型，而类型别名不仅仅为对象指定类型，实际上可以为任意类型指定别名

**接口继承**

```ts
interface point2D {
    x: number;
    y: number;
}

// 使用 extends 关键字继承
interface point3D extends point2D {
    z: number;
}
```

#### 元组类型

当记录一组数据时，可以使用数组 `Type[]` 的形式，相比之下使用元组（另一种类型的数组） `[Type1, Type2...]` 可以标记有多少的元素，和元素类型

```ts
// 数组   
let arr: number[] = [1, 2]

// 元组
let arr: [number, number] = [1, 2]
```

#### 类型推论

在TS中，某些没有明确指定类型的地方，会借助类型推论机制帮助提供类型，一般在声明变量初始化和决定函数返回值时可以不写类型注解

```ts
let age
function add(num1: number, num2: number) {
	return num1 + num2
}
```

#### 类型断言

当需要的类型注解相比 TS 给的类型更加的明确，使用类型断言来指定更加明确的类型（浏览器中选中需要的标签，在控制台通过 `console.dir($0)` 打印对象中有标签的具体类型）

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220715062028.png)

通过 `as` 关键字实现类型断言 `as` 关键字后面是类型注解的子类型

```ts
// getElementById 方法返回的 HTMLElement 类型是只包含公共属性的，不包含特有的 href 等属性
const aLink = document.getElementById('link')  
// HTMLElement 类型非常的宽泛


// 使用类型断言制定更加具体的类型
const aLink = document.getElementById('link') as HTMLAnchorElement;
// 可以使用尖括号（不常用）
const aLink = <HTMLAnchorElement>document.getElementById('link')
```

#### 字面量类型

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220715063949.png)

```ts
let a = 'a'
const b = 'b'
// 变量 a 的数据类型是 string
// 变量 b 的数据类型是 'b'

const b: 'b' = 'b'
```

a 是一个变量，他的值可以是任意的字符串，所以类型为 string
b 是一个常量，他的值不能变化，只能是 'b' 所以它的类型为 'b'

此时的 `'b'` 就是一个字面量类型，除了字符串外，任意的 JS 字面量（对象、数字等）都可以作为类型使用

字面量类型一般用于明确的表名类型的可选值，一般是与联合类型一起使用

```ts
// 指定变量 direction 只能选择这几个变量类型
function func(direction: "up" | "down" | "left" | "right" ) {
    console.log(direction);
}
```


#### 枚举类型

枚举类似于字面量类型 + 联合类型，也可以表示一组明确的可选值

枚举：定义一组命名常量。它描述一个值，该值可以是这些命名常量中的一个

```ts
// 使用 enum 关键字定义枚举  （约定枚举值以大写字母开头）
enum direction { Up, Down, Left, Right }

function move(direction: direction): void {
    console.log(direction);
}

move(direction.Up);
```

（数字枚举）枚举的成员值为数值型默认为 0 ，也可以在定义枚举时，给值赋予初始值

```ts
// Up -> 10   Down -> 11  Left -> 12   Right -> 13
enum direction { Up=10, Down, Left, Right }
// Up -> 1    Down -> 3   Left -> 5    Right -> 7
enum direction { Up=1, Down=3, Left=5, Right=7 }
```

（字符串枚举）不能自增的，所以每个成员必须要规定初始值

```ts
enum direction { 
	Up='Up', 
	Down='Down', 
	Left='Left', 
	Right='Right' 
}
```

枚举类型不仅仅是类型，也提供值，其他的类型在编译为 JS 时会被自动移除，但是枚举类型会被编译为 JS 代码

上述的字符串枚举，编译为 JS 是

```js
var direction;
(function (direction) {
	direction['Up'] = 'Up';
	direction['Down'] = 'Down';
	direction['Left'] = 'Left';
	direction['Right'] = 'Right';
})(direction || (direction = {}));
```

#### any 类型

any 类型会失去 TS 的类型保护的优势，再去操作此变量不会提示类型问题

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220718155911.png)

一般在避免较长的类型注释时，临时使用

```ts
let obj: any = { x: 0 }

obj.bar = 100
obj()
const n: number = obj
```



#### typeof 运算符

TS 中的 typeof 运算符：可以在类型上下文中引用变量或属性的类型（类型查询）

```ts
let p = { x: 1, y: 2 }
function Point( point: typeof p ) {}
```

`typeof` 只能用于查询变量或属性的类型，无法查询其他形式的类型（函数调用类型）



## TS 高级类型

### class 类

#### 基本使用

TS 全面支持 ES6 中的 class 关键字，并为其添加了类型注解和其他语法（可见性修饰符等）

定义一个类，成员变量要么初始化赋值，要么在构造函数中赋值

```ts
class person {
    age: number;   // 声明成员，没有初始化（需要在构造函数中赋值）
    name = "zs";   // 声名成员，并且初始化
}
  
const p = new person();
```

#### 构造函数

在类中需要使用构造函数来初始化类中的变量

```ts
class person {
    Age: number;
    Name: string;
    // 构造函数  constructor  构造函数不需要指定返回参数类型
    constructor(age: number, name: string) {
	    // 将外部传参传给内部的 age
        this.Age = age;
        // 只有成员是类型注解才可以通过 this 访问成员变量
        this.Name = name;
    }
}

// 在创建实例的时候传入参数
const p = new person(18, 'zs');
// 可以通过 实例.属性 的形式访问变量
p.Age = 19;
console.log(p);   // person { Age: 19, Name: 'zs' }
```


#### 实例方法

同样的 TS 的类中也有实例的方法

```ts
class Point {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
    // 定义实例方法，与函数使用方法相同
    scale(n: number): void {
        this.x *= n;
        this.y *= n;
    }
}
  
const p = new Point(1, 2);
// 调用实例方法
p.scale(2);
console.log(p);   // Point { x: 2, y: 4 }
```

#### 类的继承

通过 `extends` 关键字实现继承

```ts
class 子类 extends 父类 {}
```

子类继承父类的所有属性和方法，同时可以定义自己的属性和方法

```ts
class Animal {
    move() {
        console.log('move');
    }
}

class Dog extends Animal {
    speak() {
        console.log('bark');
    }
}

const dog = new Dog();
dog.move();
dog.speak();
```

通过 `implements` （实现接口）

```ts
class 类 implements 接口 {}
```

```ts
// 创建接口
interface Singable {
	sing(): void
}

// 实现接口，在类中必须提供接口中指定的方法和属性
class Person implements Singable {
	sing() {
		console.log('理想三旬')
	}
}
```

#### 修饰符

控制类中的属性和方法是否对外可访问

* public：公有（默认），所有可访问
	```ts
	class Person {
		// 公有的 
	    public Age: number;
	    constructor(age: number) {
	        this.Age = age;
	    }
	}
	```
* protected：受保护的，仅在类和子类中可访问，子类可以通过this访问
	```ts
	class Person {
	// 只能在类中或子类中访问
	    protected sayHello() {
	        console.log("Hello ");
	    }
	}
	  
	class Jack extends Person {
	    say() {
		    // 采用 this 关键字访问
	        this.sayHello();
	    }      
	}
	```
* private：私有的，仅在类中可调用
	```ts
	class Person {
	    private sayGoodbye() {
	        console.log("Goodbye ");
	    }
	    say() {
		    // 在类中调用
		    this.sayGoodbye()
	    }
	}
	```
* readonly：只读，只能修饰属性，防止在构造函数外对属性进行赋值
	```ts
	class Person {
		// 接口或者 {} 表示的对象类型也可以用 readonly 修饰
	    readonly name: string = 'zs';  // 允许初始化默认值
	    // 只能在构造方法中赋值
	    constructor(name: string) {
	        this.name = name;
	    }
	    
		setName() {
	        // error: 无法分配到 'name' ，因为它是只读的
	        this.name = "";
	    }
	}


	// 接口中使用
	interface Person {
	    readonly firstName: string;
	}
	// 在对象中使用
	let obj: { readonly name: string } = { name: "John" };
	```


### 类型兼容性

#### 两种类型系统

结构化类型系统（Structural Type System）、表明类型系统（Nominal Type System）

TS 采用结构化类型系统，也叫鸭子类型，类型检查关注的是值所具有的形状，例如：如果两个对象具有相同的结构，则认为它们属于同一类型

```ts
class Point2D {x: number; y: number;}
class Point {x: number; y: number;}
  
const p: Point = new Point2D();
```

两个不同的类，但是它们的结构相同（都具有 x 和 y 两个属性，属性类型也相同）所以在创建一个 Point2D 的实例时，可以采用 Point 标注类型，但是在标明类型系统中（C#、java等）他们是不同的类，类型无法兼容

#### 类之间

在上述代码之上，对象的 `point2D`  的属性结构被对象 `point3D` 所包括, 那么可以认为 `point2D` 兼容 `point3D` , 那么成员多的对象 `point3D` 可以赋值给 成员少的 `point2D`

```ts
class Point2D {x: number; y: number;}
class Point3D {x: number; y: number; z: number}
  
const p: Point2D = new Point3D();
```

#### 接口之间

接口之间的兼容性类似类之间的兼容性, 并且 `class` 和 `interface` 之间也可以兼容

```ts
interface Point {
    x: number;
    y: number;
}
  
interface Point2D {
    x: number;
    y: number;
}
  
interface Point3D {
    x: number;
    y: number;
    z: number;
}

class Point4D {
    x: number;
    y: number;
    z: number;
    w: number;
}
  
let p1: Point = { x: 1, y: 2 };
let p2: Point2D = { x: 1, y: 2 };
let p3: Point3D = { x: 1, y: 2, z: 3 };
let p4: Point3D = new Point4D();

p1 = p2;
p2 = p3;
p1 = p3;
```

不论是接口还是类，都是在约束对象，对于对象而言，参数多的可以赋值给参数少的

#### 函数之间

函数之间兼容性需要考虑到

* 参数个数
	* 参数多的兼容参数少的（少的可以赋值给多的）
	```ts
	let F1 = (a: number) => 0;
	let F2 = (a: number, b: number) => 0;
	  
	F2 = F1;
	F1 = F2; // error

    // 例如
    let a = [1, 2, 3];
  
	a.forEach((value, index, array) => {
	    console.log(value, index, array);
	});
	  
	a.forEach((value) => {
	    console.log(value);
	});

    // forEach 第一个参数是回调函数，允许三个参数，但是致谢一个参数也是没有问题的
	```
* 参数类型
	* **相同位置**的参数类型要相同或兼容
* 返回值类型
	* 对于仅仅是返回值类型不同的函数，源函数返回值类型必须是目标类型的子类型
	```TS
	let x = () => ({name: 'Alice'}); 
	let y = () => ({name: 'Alice', location: 'Seattle'}); 
	x = y; // OK 
	y = x; // Error, because x() lacks a location property
	```


### 交叉类型

交叉类型是将多个类型合并为一个类型。 这让我们可以把现有的多种类型叠加到一起成为一种类型，它包含了所需的所有类型的特性

```ts
interface Person {
    name: string;
}
  
interface Contact {
    phone: string;
}

// PersonDetails 具备 Person 和 Contact 所有属性和方法
type PersonDetails = Person & Contact;
  
let obj: PersonDetails = {
    name: 'John',
    phone: '12345'
}
```

继承 `extends` 和交叉类型再实现类型组合时，对于同名属性之间，处理类型冲突方式不同，继承中同属性名，不同类型会报错，再交叉类型中，会当作联合类型处理

```ts
interface Person {
    fun: (v: string) => string;
}
  
interface Contact extends Person {    // error: 属性的类型不兼容
    fun: (v: number) => string;
}
  
type a = Person & Contact;   // fun: (v: number | string) => string;
```

### 泛型和 keyof

#### 泛型

在保证类型安全（不丢失类型消信息）前提下，让函数等与多种类型一起工作，从而实现**复用**，常用于函数、接口、类

```ts
// 需求：传入什么类型的参数，返回什么类型的此参数

// 只能实现数字，如果要实现多个，要使用any，数据不安全，使用交叉类型过长
function id(value: number): number {
    return value;
}
```

**泛型函数**

```ts
// 泛型函数   Type（自定义名字）相当于一个类型容器，能够捕获用户类型
function id<Type>(x: Type): Type {
    return x;
}

// 函数的调用  id<type>(value)
const str = id<string>("string");   // 指定类型
const num = id(1)      // 简化，通过类型参数推断机制推断类型（有时会推断类型不准确）
```

**泛型约束**

泛型函数的变量可以是很多类型，而有些类型拥有的方法和属性，在其他类型中没有，着用啊会导致无法使用方法，或调用属性。此时，就需要添加约束来收缩类型（缩窄类型取值范围）

```ts
function getLen<T>(value: T): T {   // 不加约束，不可以访问
    return value.length;   // error：类型 T 不存在属性 length
}

// 1、直接指定具体的类型
function getLen<T>(value: T[]): T[] {  // 指定类型为数组
    console.log(value.length);
    return value;
}

// 2、添加约束
// 撞见描述约束的接口，要求提供length属性
interface iLength {
    length: number;
}
  
// 此时的 extends 理解为 要求参数必须符合接口的要求（必须要有length属性）
function getLen<T extends iLength>(value: T): T {
    console.log(value.length);
    return value;
}
```

泛型的类型变量可以有多个，并且多个变量之间可以约束（第二个变量约束第一个变量）

```ts
// 类型变量 K 受类型参数 T 的限制
function get<T, K extends keyof T>(obj: T, key: K) {
    return obj[key];
}
  
// keyof 接收一个对象类型，生成其所有键的（string或number）联合类型（集合）
  
let person = {
    name: 'jack',
    age: 18,
    190: '190'
}

// 示例联合类型为 'name' | 'age' | 190
  
console.log(get(person, 'name'));
console.log(get(person, 190));
// 可以传入其他对象，访问其方法，索引、属性
console.log(get('abc', 'length'));
console.log(get('abc', 1));   // 此处 1 代表索引

// 一般用来访问对象，不用于其他的
```

**泛型接口**

泛型接口：在接口名称的后面添加类型变量

接口的类型变量对接口中的所有成员可见，所有成员都可以使用

```ts
interface IdFunc<T> {
    id: (value: T) => T;
    ids: () => T[];
}
```

在使用泛型接口时，需要显示指定具体的类型（没有类型推断）

```ts
// 指定接口的及具体类型
let obj: IdFunc<number> = {
    id(value) {return value},
    ids: () => [1, 2, 3]
}
```

>数组是泛型接口

**泛型类**

与泛型接口不同的是，使用类时可以不用传入类型

```ts
class NumberClass<NumType> {
    defaultValue: NumType;   
    add: (a: NumType, b: NumType) => NumType;
    constructor(value: NumType) {   // 赋值（如果没有，那么最好指定类型）
	    this.defaultValue = value
    }
}
  
const myNum = new NumberClass(100);   // 此时可以省略
myNum.defaultValue = 10;
```

**泛型工具类型**

ts中有一些内置的常用的工具类型，来简化一些长技安的操作

* `Partial<Type>` ：用来构造一个类型，将 Type 的所有属性设置为可选的
	```ts
	interface Props {
	    name: string;
	    age: number;
	}
	  
	// p 类型拥有 Props 类型的所有属性，且属性都是可选的
	type P = Partial<Props>;
	```
* `Readonly<Type>`：用来构造一个类型，将 Type 的所有属性设置为只读
	```ts
	interface Props {
	    name: string;
	    age: number;
	}
	  
	// p 类型拥有 Props 类型的所有属性，且属性都是只读的
	type p = Readonly<Props>;
	const a: p = {
	    name: '张三',
	    age: 18
	};
	  
	a.name = '李四'; // error: 只读类型不能修改属性
	```
* `Pick<Type, Keys>`：从 Type 中选择一组属性类构造新类型
	```ts
	interface Props {
	    name: string;
	    age: number;
	    address: string;
	}
	  
	// p 类型拥有 Props 类型的部分属性
	type p = Pick<Props, 'name' | 'age'>;
	  
	const a: p = {
	    name: '张三',
	    age: 18
	}
	```
* `Record<Keys, Type>`：构造一个对象，属性类型为 Type
	```ts
	interface obj {
	    name: string;
	    age: number;
	    address: string;
	};
	  
	// 此类型拥有 obj 的所有属性，属性类型指定为 string
	type o = Record<keyof obj, string>;   // 利用 keyof 获取所有键
	// 此类型拥有如下属性：name、age、address
	type p = Record<'name' | 'age' | 'address', string>;
	```

### 索引签名类型

当无法确定对象中有那些属性时，可以使用索引前面类型

```ts
interface Arr<T> {
    [index: number]: T;   // 可以拥有无数个 index: T
} 

let arr: Arr<number> = [1, 2, 3]
```

### 映射类型

映射类型：基于旧类型，创建新类型
```ts
type keys = 'x' | 'y' | 'z';
// 依据 keys 类型 创建新类型
type obj = { [K in keys]: number };
```

>只能在类型别名中使用，不能再接口中使用

使用 keyof 
```ts
type obj = {a: number, b: string, c: boolean};
type obj2 = { [K in keyof obj]: number };
```

**Partial\<type>**  的实现

```ts
type Partial<T> = {
    [P in keyof T]?: T[P];
}
```

T[P]：获取T中每个键对应的类型（[索引查询类型](###索引查询类型)）


### 索引查询类型

用于查询索引的属性的类型

```ts
type obj = {
    name: string;
    age: number;
    address: string;
}
  
type a = obj['name'];   // 如果直接写，会被判断为 js 中的访问属性
type b = obj[keyof obj];   // string | number
type c = obj['name' | 'age' | 'address'];   // string | number
```

## 类型声明文件

### 概述

ts 最后的代码是需要编译成 js 的代码，此时的 js 依旧有相应的 ts 类型，这来源于**类型声明文件**，用来为已存在的 JS 库提供类型信息，这样的 TS 项目中使用这些 JS 库，也会有代码提示和类型保护

### 两种文件类型

TS 中有两种文件类型：
* .ts （代码实现文件）
	* 既包含类型信息又可执行代码
	* 可以被编译为 .js 文件，然后执行
* .d.ts（类型声明文件）
	* 只包含类型信息的类型声明文件
	* 不会生成 js 文件，仅用于提供类型信息
	* 用于为 js 文件提供类型信息

### 类型声明文件的使用

内置类型声明文件：所有的标准化 API 、BOM、DOM等都提供了声明文件

第三方库类型声明文件：基本所有常用的第三方库都有类型声明文件，库自带声明文件或由 DefinitelyTyped （github）提供

在 `package.json` 中使用`types`或`typeing`属性指定需要读取的类型声明文件

个人项目如果想在项目内共享类型或为已有的js文件提供类型声明，可以创建类型声明文件

项目内共享类型（多个ts文件用到同一个类型）
```ts
// Props.d.ts
type Props = { x: number, y: number };
  
export {
    Props
}

// index.ts
import { Props } from './Props'  // 不需要后缀
```

为已有的js添加声明

1、js项目迁移ts，为原来js编写类型声明

通过 ts-loader 处理 .ts文件`npm install ts-loader -D`

基于 ES6 模块化方案创建类型声明文件

使用 webpack ，还需要再更目录创建 `tsconfig.json` ts配置文件