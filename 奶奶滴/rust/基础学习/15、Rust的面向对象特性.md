## 面向对象语言的特点

**Rust受到多种编程范式的影像，也包括面向兑现**

面向对象通常包含以下特征：命名对象、封装、继承

### 对象包含数据和行为

- 面向对象的程序由对象组成
- 对象包装了数据和操作这些数据的过程，通常称为方法

>基于上述定义：Rust是面向对象的。构体和枚举包含数据而 `impl` 块提供了在结构体和枚举之上的方法。虽然带有方法的结构体和枚举并不被 **称为** 对象，但是他们提供了与对象相同的功能

### 封装

- 对象的实现细节不能被使用对象的代码获取到，唯一与对象交互的方式是通过对象提供的公有 API

Rust：使用 `pub` 关键字来决定模块、类型、函数和方法是公有的，而默认情况下其他一切都是私有的

### 继承

使一个对象可以获得另一个对象的数据和行为，而无需重新定义

Rust是没有继承的

一般来说使用继承的原因如下
- 代码复用
	- rust中采用 `trait` 方法来实现代码共享
- 多态
	- rust采用泛型和`trait`约束（限定参数化多态）


## 使用trait对象存储不同类型值

在rust中称呼`trait`为对象，而不是称呼`struct`、`enum`为对象

现在创建我们需要创建一个`Gui`工具，他拥有一个列表，保存着所有的元素，这些元素上都拥有一个`draw`方法，用来绘画出此元素

那么在其他的面向对象编程语言中，我们可以先定义一个父类 `Component` ，父类拥有 `draw` 方法，然后其他子元素会继承父类，并实现 `draw` 方法。

但是在rust中没有继承，我们依旧可以通过`trait`实现

```rust
pub trait Draw {
	fn draw(&self);
}
  
pub struct Screen {
	pub components: Vec<Box<dyn Draw>>,
}
  
impl Screen {
	pub fn run(&self) {
		for component in self.components.iter() {
			component.draw();
		}
	}
}
```

上述中，因为我们需要将所有的元素放到 `Vec` 中，所以我们需要使用 `Box<dyn Draw>` 来指定所有实现了 `Draw` 的类型

如果我们像如下使用了泛型 `T` 泛型类型参数一次只能替代一个具体类型，也就是说，当我们创建一个 `Screen` 时，传入的第一个类型，会被当作`T`，那么后面其他元素也必须是这个类型


```rust
pub trait Draw {
	fn draw(&self);
}
  
pub struct Screen<T>
where
	T: Draw,
{
	pub components: Vec<T>,
}
  
impl<T> Screen<T>
where
	T: Draw,
{
	pub fn run(&self) {
		for component in self.components.iter() {
			component.draw();
		}
	}
}
```


定义两个元素

```rust
// src/lib.rs
pub trait Draw {
	fn draw(&self);
}
  
pub struct Screen {
	pub components: Vec<Box<dyn Draw>>,

}
  
impl Screen {
	pub fn run(&self) {
		for component in self.components.iter() {
			component.draw();
	   }
	
	}
}

  
pub struct Button {
	pub width: u32,
	pub height: u32
	
	pub label: String,
}
  
impl Draw for Button {
	fn draw(&self) 
	
		// code to actually draw a button
		println!("Button draw");
	}
}
  
pub struct SelectBox {
	pub width: u32,
	pub height: u32,
	pub options: Vec<String>,
}
  
impl Draw for SelectBox {
	fn draw(&self) {
		// code to actually draw a select box
		println!("SelectBox draw");
	}
}
```

在main函数中调用

```rust
use my_gui::{SelectBox, Screen, Button};

fn main() {
	let btn = Button {   
		width: 10,
		height: 10,
		label: String::from("Click me!"),
	};
	
	let select_box = SelectBox {
		width: 10,
		height: 10,
		options: vec![String::from("Yes"), String::from("Maybe"), String::from("No")],
	};
	
	let screen = Screen {
		components: vec![Box::new(btn), Box::new(select_box)], 
	};
	
	screen.run();
}
```

当编写库的时候，我们不知道何人何时会添加一个`Box`或者`Button`，但是我们可以肯定的是，这些类型一定都是实现了`draw`

这个概念 —— 只关心值所反映的信息而不是其具体类型 —— 类似于动态类型语言中称为 **鸭子类型**（_duck typing_）的概念：如果它走起来像一只鸭子，叫起来像一只鸭子，那么它就是一只鸭子！

例如我们只要实现了 `Draw` 那么就可以传入 `Screen` 中，并成功运行

```rust
struct S {
	msg: String,
}
  
impl Draw for S {
	fn draw(&self) {
		println!("{}", self.msg);
	}
}

fn main() {
	let s = S {
		msg: String::from("Hello world!"),
	};
	
	let screen = Screen {
		components: vec![Box::new(s)],  // Hello world!
	};
	
	screen.run();
}
```

### trait对象执行动态分发

- 将trait约束作用于泛型时，Rust会执行单态化
 - 编译器为每一个被泛型类型参数代替的具体类型生成了函数和方法的非泛型实现
- 通过单态化的代码会被执行静态派发，在编译过程中确定调用的具体方法
- 动态派发
	- 无法在编译过程中确定调用哪一个方法
	- 编译器会产生额外代码以便在运行时找出希望调用的方法
- 使用trait对象，会执行动态派发
	- 产生运行时开销
	- 组织编译器内联方法代码，使得部分优化操作无法进行


### Trait对象必须保证对象安全

- 只有对象安全（object-safe）的 trait 可以实现为特征对象
- rust采用多个规则来判断某个对象是否安全（只需记两条）
	- 方法返回类型不是 `Self`
	- 方法中不包含任何泛型类型参数


## 实现面向对象的设计模式

### 状态模式

- 定义一系列值的内含状态。这些状态体现为一系列的 **状态对象**，同时值的行为随着其内部状态而改变


使用状态模式意味着：程序的业务需求改变时，无需改变值持有状态或者使用值的代码。我们只需更新某个状态对象中的代码来改变其规则，或者是增加更多的状态对象


例如：发布博客
1.  博文从空白的草案开始。
2.  一旦草案完成，请求审核博文。
3.  一旦博文过审，它将被发表。
4.  只有被发表的博文的内容会被打印，这样就不会意外打印出没有被审核的博文的文本。

通过上述需求，可以发现如下：
- 1、任何一篇文章都有三个状态：草案（未完成）、待审核（已完成）、审核通过（发表）
- 2、对于一个状态，有如下两个选择：
	- 1、是否可以审核
	- 2、是否可以发布
- 3、一篇文章拥有两个属性，三个方法
	- 状态属性、文本内容
	- 编写文章（add）
	- 请求审核
	- 发布文章

所以我们需要定义一个`post`结构体，存放文章。然后还需要定义状态`State` trait 包含审核、发布两个接口，并且为三种状态`Draft`、`PendingReview`、`Published`实现这两个接口。最后为`post`实现三个方法

```rust
pub struct Post {
	state: Option<Box<dyn State>>,
	content: String,
}

impl Post {
	pub fn new() -> Post {
		Post {
			state: Some(Box::new(Draft {})),
			content: String::new(),
		}
	}
  
	// 添加文本
	pub fn add_text(&mut self, text: &str) {
		// self.content.push_str(text);
		self.content.push_str(text);
	}
  
	// 请求审核
	pub fn request_review(&mut self) {
		// self.state = Some(Box::new(PendingReview {}));
		if let Some(s) = self.state.take() {
			self.state = Some(s.request_review())
		}
	}
  
	// 审核通过，发布
	pub fn approve(&mut self) {
		// self.state = Some(Box::new(Published {}));
		if let Some(s) = self.state.take() {
			self.state = Some(s.approve())
		}
	}
}
  
trait State {
	fn request_review(self: Box<Self>) -> Box<dyn State>;
	fn approve(self: Box<Self>) -> Box<dyn State>;
}   // 状态
  
struct Draft {}  // 草稿
  
impl State for Draft {
	fn request_review(self: Box<Self>) -> Box<dyn State> {
		Box::new(PendingReview {})
	}
	fn approve(self: Box<Self>) -> Box<dyn State> {
		self
	}
}   // 草稿状态下，可以请求审核
  
struct PendingReview {}    // 待审核
  
impl State for PendingReview {
	fn request_review(self: Box<Self>) -> Box<dyn State> {
		self
	}
	fn approve(self: Box<Self>) -> Box<dyn State> {
		Box::new(Published {})
	}
}   // 待审核状态下，可以请求审核，也可以发布
  
struct Published {}    // 已发布
  
impl State for Published {
	fn request_review(self: Box<Self>) -> Box<dyn State> {
		self
	}
	fn approve(self: Box<Self>) -> Box<dyn State> {
		self
	}
}   // 已发布状态下，不可以请求审核，也不可以发布
```

但是上述结构任有缺陷，我们在发布状态时，是可以获取到`content`的。然而这个方法不能直接在`Post`上实现，而是需要与状态相关，所以我们需要在`State`上默认实现`content`

```rust
trait State {
	// 因为返回值可能是 post 的一部分，所以返回值生命周期与 post 是关联的
	fn content<'a>(&self, post: &'a Post) -> &'a str {
		""
	}
}   // 状态

// 仅在发布状态下实现content
impl State for Published {
	fn content<'a>(&self, post: &'a Post) -> &'a str {
		&post.content
	}
}
```

在 `post` 中使用方法调用

```rust
impl Post {
	// 获取内容
	pub fn content(&self) -> &str {
		// as_ref() 方法将 Option<T> 转换为 Option<&T>，这样就可以调用 content 方法了
		self.state.as_ref().unwrap().content(&self)
	}
}
```

上述就是一个基本的状态模式示例

显然，状态模式会导致某些状态之间是相互耦合的，需要重复实现一些逻辑代码

### 将状态和行为编码为类型

不同于完全封装状态和状态转移使得外部代码对其毫不知情，我们将状态编码进不同的类型。如此，Rust 的类型检查就会将任何在只能使用发布博文的地方使用草案博文的尝试变为编译时错误

不同于上述的方式，我们创建两个结构体，一个代表发表的博文，一个代表未发表的博文。那么对于未发表的博文，我们不给予 `content` 方法，那么当调用时就会直接报错（对于不同的状态的博文实现不同的方法）

```rust
pub struct Post {  // 发布博文
    content: String,
}

pub struct DraftPost {   // 草稿博文
    content: String,
}

impl Post {
    pub fn new() -> DraftPost {  // 实例化一个草稿
        DraftPost {
            content: String::new(),
        }
    }

    pub fn content(&self) -> &str {  // 可以获取文章
        &self.content
    }
}

impl DraftPost {
    pub fn add_text(&mut self, text: &str) {  // 草稿可以添加文章
        self.content.push_str(text);
    }
    
	pub fn request_review(self) -> PendingReviewPost {  // 可以审批
		PendingReviewPost {
			content: self.content,
		}
	}
}
  
pub struct PendingReviewPost {  // 审核博文
	content: String,
}
  
impl PendingReviewPost {
	pub fn approve(self) -> Post {  // 仅仅可以提交审核
		Post {
			content: self.content,
		}
	}
}
```

**对于不同的状态实现他们所对应的方法**

```rust
use blog::Post;
  
fn main() {
	let mut post = Post::new();  // 此时返回的是 草稿post
	post.add_text("I ate a salad for lunch today");
	let post = post.request_review();  // 此时是 审核post
	let post = post.approve();  // 此时时 发布post
	
	assert_eq!("I ate a salad for lunch today", post.content());
}
```

>Rust 不仅能够支持面向对象的设计模式，还可以支持更对的模式