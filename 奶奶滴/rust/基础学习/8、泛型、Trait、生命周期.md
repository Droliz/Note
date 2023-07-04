## 泛型

### 结构体泛型

```rust
struct A<T> {
	x: T,
}
```


### 枚举泛型

```rust
enum Option<T> {
    Some(T),
    None,
}
```

### 方法泛型

```rust
impl<T> A<T> {
	fn a(&self) -> &T {
		&self.x
	}
}
```

**注意：** `A<T>` 是结构体的名字

## trait

### 基本使用

trait 定义是一种将方法签名组合起来的方法，目的是定义一个实现某些目的所必需的行为的集合（接口）

例如：将`toString、push、pop`等方法使用`trait`定义，在需要实现这些方法的结构体中使用`impl for`来实现方法

```rust
pub trait Summary {
	fn summarize(&self) -> String;  // summarize 方法
}
  
pub struct Tweet {
	pub username: String,
	pub content: String,
	pub reply: bool,
	pub retweet: bool,
}
  
impl Summary for Tweet {
	fn summarize(&self) -> String {  // 实现方法
		format!("{}: {}", self.username, self.content)
	}
}
  
fn main() {
	let tweet = Tweet {
		username: String::from("horse_ebooks"),
		content: String::from("of course, as you probably already know, people"),
		reply: false,
		retweet: false,
	};
	println!("{}", tweet.summarize());
}
```

### 引入

当引入的结构体中的方法包含了 `trait` 的实现，那么引入时也需要引入 `trait` ，并且这个 `trait` 需要 `pub`

```rust
// dome/src/main.rs
use demo::{Summary, Tweet};
  
fn main() {
	let tweet = Tweet {
		username: String::from("horse_ebooks"),
		content: String::from("of course, as you probably already know, people"),
		reply: false,
		retweet: false,
	};
	println!("{}", tweet.summarize());
}
```

```rust
// dome/src/lib.rs
pub trait Summary {
    fn summarize(&self) -> String;
}
  
pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}
  
impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}
```

### trait作为参数

在上述中，定义了一个 `trait` ，在 `Tweet` 中实现了该 `trait`。可以将 `trait` 作为参数，限制参数必须实现此 `trait`

```rust
pub fn notify(item: &impl Summary) { 
	println!("Breaking news! {}", item.summarize()); 
}
```

>item必须实现`Summary`，不论item什么类型

**trait bound**

对于上述的代码可以使用 `trait bound` 的方式简化

```rust
pub fn notify<T: Summary>(item: &T) { 
	println!("Breaking news! {}", item.summarize()); 
}
```

对于多个参数，如果仅仅使用 `impl trait` 那么参数是可以不同类型的，如果需要参数的类型也一致，那么就必须使用 `trait bound` 的形式

```rust
pub fn notify<T: Summary>(item1: &T, item2: &T) {}  // item1、item2的类型也必须一致
```

**指定多个trait**

当需要参数同时实现两个 `trait` 时，使用 $+$ 连接

```rust
pub fn notify(item: &(impl Summary + Display)) {}
```

如果是 $+?$ 代表只要有一个就可以，不管是哪一个

语法糖写法

```rust
pub fn notify<T: Summary + Display>(item: &T) {}
```

**where简化**

当 `trait` 约束多了，使用语法糖依旧会显得臃肿，可以使用 `where` 简化

```rust
fn some_function<T: Display + Clone, U: Clone + Debug>(t: &T, u: &U) -> i32 {}
```

使用 `where` 简化

```rust
fn some_function<T, U>(t: &T, u: &U) -> i32
where
    T: Display + Clone,
    U: Clone + Debug,
{
    unimplemented!()
}
```

### 返回实现trait的类型

使用 `impl trait` 返回实现了 `trait` 的类型

```rust
fn returns_summarizable() -> impl Summary {  // 返回类型必须实现 Summary
    Tweet {
        username: String::from("horse_ebooks"),
        content: String::from(
            "of course, as you probably already know, people",
        ),
        reply: false,
        retweet: false,
    }
}
```

**注意：** 返回值类型指定为返回 `impl Summary`，但是返回了 `NewsArticle` 或 `Tweet` 就行不通。只能返回其中一个

```rust
fn returns_summarizable(switch: bool) -> impl Summary {
	if switch {
		NewsArticle {
			headline: String::from(
				"Penguins win the Stanley Cup Championship!",
			),
			location: String::from("Pittsburgh, PA, USA"),
			author: String::from("Iceburgh"),
			content: String::from(
			   "The Pittsburgh Penguins once again are the best \
				 hockey team in the NHL.",
			),
		}
	} else {
		Tweet {
			username: String::from("horse_ebooks"),
			content: String::from(
				"of course, as you probably already know, people",
			),
			reply: false,
			retweet: false,
		}
	}
}
```

上述代码会报错，因为只允许返回一种

**使用trait bound有条件实现方法**

通过使用带有 trait bound 的泛型参数的 `impl` 块，可以有条件地只为那些实现了特定 trait 的类型实现方法

```rust
use std::fmt::Display;
  
struct Pair<T> {
    x: T,
    y: T,
}
  
impl<T> Pair<T> {
    fn new(x: T, y: T) -> Self {
        Self { x, y }
    }
}
  
impl<T: Display + PartialOrd> Pair<T> {
    fn cmp_display(&self) {
        if self.x >= self.y {
            println!("The largest member is x = {}", self.x);
        } else {
            println!("The largest member is y = {}", self.y);
        }
    }
}

  
fn main() {
    let pair = Pair::new("word", "new");
    pair.cmp_display();
    let pair = Pair::new(1, 2);
    pair.cmp_display();
}
```

对于上述代码，`Pair` 的参数类型必须实现了 `Display` 和 `PartialOrd`才回去实现 `cmp_display` 方法。例如上述对于字符串以及`i32`都是实现了`display`以及`partialOrd`方法，所以可以实现 `cmp_display` 方法

## 生命周期

### 简介

Rust 中的每一个引用都有其 **生命周期**（_lifetime_），也就是引用保持有效的作用域。大部分时候生命周期是隐含并可以推断的，正如大部分时候类型也是可以推断的一样。

类似于当因为有多种可能类型的时候必须注明类型，也会出现引用的生命周期以一些不同方式相关联的情况，所以 Rust 需要我们使用泛型生命周期参数来注明他们的关系，这样就能确保运行时实际使用的引用绝对是有效的

>生命周期的主要目标是避免**悬垂引用**（_dangling references_），后者会导致程序引用了非预期引用的数据

### 借用检查器

Rust 编译器有一个 **借用检查器**（_borrow checker_），它比较作用域来确保所有的借用都是有效的

```rust
fn main() {
    let r;                // ---------+-- 'a
                          //          |
    {                     //          |
        let x = 5;        // -+-- 'b  |
        r = &x;           //  |       |
    }                     // -+       |
                          //          |
    println!("r: {}", r); //          |
}                         // ---------+
```

其中 `'a、'b` 分别是 `r、x` 的生命周期，由于上述 `x` 的生命周期要小于 `r` 的，所以在编译时，会显示 `r` 借用 `x` 是不被允许的（引用了一个被销毁的变量）

我们需要将 `x` 得生命周期不能比 `r` 得生命周期短

### 函数中的泛型生命周期

```rust
fn main() {
	let string1 = String::from("abcd");
	let string2 = "xyz";
	
	let result = longest(string1.as_str(), string2);
	println!("The longest string is {}", result);
}
  
fn longest(x: &str, y: &str) -> &str {
	if x.len() > y.len() {
		x
	} else {
		y
	}
}
```

当我们定义这个函数的时候，并不知道传递给函数的具体值，所以也不知道到底是 `if` 还是 `else` 会被执行。我们也不知道传入的引用的具体生命周期

借用检查器自身同样也无法确定，因为它不知道 `x` 和 `y` 的生命周期是如何与返回值的生命周期相关联的。为了修复这个错误，我们将增加泛型生命周期参数来定义引用间的关系以便借用检查器可以进行分析

### 生命周期注解

生命周期注解并不改变任何引用的生命周期的长短。

相反它们描述了多个引用生命周期相互的关系，而不影响其生命周期。

与当函数签名中指定了泛型类型参数后就可以接受任何类型一样，当指定了泛型生命周期后函数也能接受任何生命周期的引用

```rust
&i32        // 引用
&'a i32     // 带有显式生命周期的引用
&'a mut i32 // 带有显式生命周期的可变引用
```

### 函数签名中得生命周期注解

为了在函数签名中使用生命周期注解，需要在函数名和参数列表间的尖括号中声明泛型生命周期（_lifetime_）参数，就像泛型类型（_type_）参数一样

```rust
fn main() {
    let string1 = String::from("abcd");
    let string2 = "xyz";

    let result = longest(string1.as_str(), string2);
    println!("The longest string is {}", result);
}

fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```


当具体的引用被传递给 `longest` 时，被 `'a` 所替代的具体生命周期是 `x` 的作用域与 `y` 的作用域相重叠的那一部分。换一种说法就是泛型生命周期 `'a` 的具体生命周期等同于 `x` 和 `y` 的生命周期中较小的那一个

所以返回值得生命周期也是较小得哪一个

那么如下例子中

```rust
fn main() {
    let string1 = String::from("long string is long");

    {
        let string2 = String::from("xyz");
        let result = longest(string1.as_str(), string2.as_str());
        println!("The longest string is {}", result);
    }
}
```

上述中，返回值得声明周期就是 `string2` 的生命周期


```rust
fn main() {
    let string1 = String::from("long string is long");
    let result;
    {
        let string2 = String::from("xyz");
        result = longest(string1.as_str(), string2.as_str());
    }
    println!("The longest string is {}", result);
}
```

上述代码是不可编译的，因为 `result` 接收返回值，它的生命周期与 `string2` 相同

### 深入理解生命周期

指定生命周期的方式依赖于函数内部的操作，上述的 `longest` 函数返回值生命周期涉及到 `x、y` 两个参数生命周期，所以需要对两个参数都指定生命周期

但是如果只与 `x` 有关，那么可以只指定 `x` 的生命周期

```rust
fn longest<'a>(x: &'a str, y: &str) -> &'a str {
    x
}
```

从函数返回引用时，返回类型的生命周期参数必须与其中一个生命周期参数匹配。如果没有指定，那么返回的内容就只能是引用的函数内部创建的值，此时就产生 _悬垂引用_（在函数结束时，走出作用域）


### 结构体中定义生命周期标注

```rust
struct ImportantExcerpt<'a> {
	part: &'a str,
}
  
fn main() {
	let novel = String::from("Call me Ishmael. Some years ago...");
	let first_setence = novel.split('.').next().expect("Could not find a '.'");
	let i = ImportantExcerpt {
		part: first_sentence,
	};
	println!("{:?}", i.part);
}
```

### 生命周期省略

函数或方法的参数的生命周期被称为 **输入生命周期**（_input lifetimes_），而返回值的生命周期被称为 **输出生命周期**

第一条规则是编译器为每一个引用参数都分配一个生命周期参数。换句话说就是，函数有一个引用参数的就有一个生命周期参数：`fn foo<'a>(x: &'a i32)`，有两个引用参数的函数就有两个不同的生命周期参数，`fn foo<'a, 'b>(x: &'a i32, y: &'b i32)`，依此类推。

第二条规则是如果只有一个输入生命周期参数，那么它被赋予所有输出生命周期参数：`fn foo<'a>(x: &'a i32) -> &'a i32`。

第三条规则是如果方法有多个输入生命周期参数并且其中一个参数是 `&self` 或 `&mut self`，说明是个对象的方法 (method)(译者注：这里涉及 rust 的面向对象参见 17 章)，那么所有输出生命周期参数被赋予 `self` 的生命周期。第三条规则使得方法更容易读写，因为只需更少的符号。


### 方法中的生命周期

```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}

impl<'a> ImportantExcerpt<'a> {
    fn level(&self) -> i32 {
        3
    }
}

impl<'a> ImportantExcerpt<'a> {
    fn announce_and_return_part(&self, announcement: &str) -> &str {
        println!("Attention please: {}", announcement);
        self.part
    }
}

fn main() {
    let novel = String::from("Call me Ishmael. Some years ago...");
    let first_sentence = novel.split('.').next().expect("Could not find a '.'");
    let i = ImportantExcerpt {
        part: first_sentence,
    };
}

```


### 静态生命周期

这里有一种特殊的生命周期值得讨论：`'static`，其生命周期**能够**存活于整个程序期间。所有的字符串字面值都拥有 `'static` 生命周期，我们也可以选择像下面这样标注出来

```rust
#![allow(unused)]
fn main() {
	let s: &'static str = "I have a static lifetime.";
}
```

### 结合泛型类型参数、trait bounds 和生命周期

```rust
fn main() {
    let string1 = String::from("abcd");
    let string2 = "xyz";

    let result = longest_with_an_announcement(
        string1.as_str(),
        string2,
        "Today is someone's birthday!",
    );
    println!("The longest string is {}", result);
}

use std::fmt::Display;

fn longest_with_an_announcement<'a, T>(
    x: &'a str,
    y: &'a str,
    ann: T,
) -> &'a str
where
    T: Display,
{
    println!("Announcement! {}", ann);
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

