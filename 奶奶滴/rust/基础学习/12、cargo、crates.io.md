## release profiles

### 基本的profile

在 Rust 中 **发布配置**（_release profiles_）是预定义的、可定制的带有不同选项的配置，他们允许程序员更灵活地控制代码编译的多种选项。每一个配置都彼此相互独立

Cargo 有两个主要的配置：运行 `cargo build` 时采用的 `dev` 配置和运行 `cargo build --release` 的 `release` 配置。`dev` 配置被定义为开发时的好的默认配置，`release` 配置则有着良好的发布构建的默认配置

```sh
$ cargo build
    Finished dev [unoptimized + debuginfo] target(s) in 0.0s
$ cargo build --release
    Finished release [optimized] target(s) in 0.0s
```

### 自定义profile

当项目的 _Cargo.toml_ 文件中没有显式增加任何 `[profile.*]` 部分的时候，Cargo 会对每一个配置都采用默认设置。通过增加任何希望定制的配置对应的 `[profile.*]` 部分，我们可以选择覆盖任意默认设置的子集

```toml
# Cargo.toml
[profile.dev]
opt-level = 0

[profile.release]
opt-level = 3
```

## 发布crate到crates.io

### 文档注释

他们会生成 HTML 文档。这些 HTML 展示公有 API 文档注释的内容，他们意在让对库感兴趣的程序员理解如何 **使用** 这个 crate，而不是它是如何被 **实现** 的

文档注释使用三斜杠 `///` 而不是两斜杆以支持 Markdown 注解来格式化文本。文档注释就位于需要文档的项的之前

```rust
/// Adds one to the number given.
///
/// # Examples
///
/// ```
/// let arg = 5;
/// let answer = my_crate::add_one(arg);
///
/// assert_eq!(6, answer);
/// ```
pub fn add_one(x: i32) -> i32 {
	x + 1
}
```

运行 `cargo doc --open` 会构建当前 crate 文档（同时还有所有 crate 依赖的文档）的 HTML 并在浏览器中打开

![](../../../markdown_img/Pasted%20image%2020230516221556.png)

同时可以看到在 `target` 目录下生成了 `doc` 文档目录

![](../../../markdown_img/Pasted%20image%2020230516221609.png)

### 常用的文档注释

除了上述的 `Examples` 区域（章节），还有如下常见的

-   **Panics**：这个函数可能会 `panic!` 的场景。并不希望程序崩溃的函数调用者应该确保他们不会在这些情况下调用此函数。
-   **Errors**：如果这个函数返回 `Result`，此部分描述可能会出现何种错误以及什么情况会造成这些错误，这有助于调用者编写代码来采用不同的方式处理不同的错误。
-   **Safety**：如果这个函数使用 `unsafe` 代码（这会在第十九章讨论），这一部分应该会涉及到期望函数调用者支持的确保 `unsafe` 块中代码正常工作的不变条件（invariants）。

### 文档注释作为测试

上述的 `Examples` 区域中的测试代码，在调用 `cargo test` 会自动进行测试

### 为包含注释的项添加文档注释

文档注释风格 `//!` 为包含注释的项，而不是位于注释之后的项增加文档。这通常用于 crate 根文件（通常是 _src/lib.rs_）或模块的根文件为 crate 或模块整体提供文档

对于上述的`lib.rs`添加如下

```rust
//! # My Crate
//!
//! `my_crate` is a collection of utilities to make performing certain
//! calculations more convenient.
  
/// Adds one to the number given.
```

用于描述包或模块的整体条目

### pub use

使用 `pub use` 可以将原本在很深层的api提取到别的地方（方便调用）

```rust
use crate::kind::aaa;
use crate::kind::bbb;
use crate::kind::xxx;

// 可以在kind中使用 pub use 将上述全部放在 kind同级
```

这样可以方便使用者查找api


### 发布

先获取账号（使用github登陆即可），获在用户设置中生成 `api token`

## cargo工作空间

**工作空间** 是一系列共享同样的 _Cargo.lock_ 和输出目录的包

### 创建工作空间

现在来组建一个工作空间：1个二进制crate，2个库crate

二进制的 `crate: main` 函数依赖于其他两个 crate

一个库提供 `add_one` 函数

一个库提供 `add_two` 函数

创建文件夹 `ADD` ，在 `ADD` 下创建 `Cargo.toml` 配置文件

```toml
[workspace]
  
members = [
    "adder"   # crate name
]
```

在 `ADD` 下创建 `adder` crate ，作为二进制 crate

使用 `cargo build` 会发现生成一个 `target` 这个是整个项目的 `target` 其他 `carte` 也会编译到此目录

在 `ADD` 下创建 `add-one、add-two` crate

```sh
cargo new add-one --lib
cargo new add-two --lib
```

然后在 `adder` 的 `Cargo.toml` 中添加依赖关系（需要显式的指明路径）

```toml
[dependencies]
add-one = { path = "../add-one" }
add-two = { path = "../add-two" }
```

在main中导入使用

```rust
use add_one;
  
fn main() {
	let num = 1;
	let result = add_one::add_one(num);
	println!("{} + 1 = {}", num, result);
}
```

