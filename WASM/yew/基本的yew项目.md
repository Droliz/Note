## 基本的yew项目

创建yew项目

```sh
cargo new yew-01 && cd yew-01
```

添加`yew`依赖

```toml
[dependencies]
yew = "0.19.3"
```

在根目录添加`index.html`文件

```html
<!DOCTYPE html>
<html lang="en">
  
<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
	<style>
		html,
		body {
			height: 100%;
			margin: 0;
			padding: 0;
			background-color: aquamarine;
		}
		.title {
			font-size: 50px;
			text-align: center;
			margin-top: 20%;
		}
	</style>
</head>
  
<body>
</body>
  
</html>
```

编写组件（见 [创建组件](./组件/创建组件.md) ）

```rust
// lib.rs
use yew::prrlude::*;

#[function_component(App)]
pub fn app() -> Html {
	
	html! {
		<div>
			<h1 class="title">{ "Hello World!" }</h1>
		</div>
	}
}
```

设置`main`入口

```rust
// main.rs
use yew_01::App;

fn main() {
	yew::start_app::<App>();
}
```

接下来使用 `trunk` 启动服务

```sh
trunk serve --open
```

需要提前下载

```sh
cargo install trunk wasm-bindgen-cli
```

