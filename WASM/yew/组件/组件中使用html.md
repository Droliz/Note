## 组件中使用html

### 简单渲染html

在rs文件中，采用`html!`宏，来编写html

```rust
html! {
	<div>
		<h1>{ "Users" }</h1>
	</div>
}
```

但是需要注意的是，需要采用`{}`包裹变量与字面量才能使用，此宏会返回一个`Html`对象

html宏与vue2的模板相似，都只能允许一个根节点。有时需要多个根节点，而且不想破坏html结构，那么可以使用空的`<></>`来包裹（**片段**）

```rust
html! {
	<>
		<h1 class="title">{ "Hello, world!" }</h1>
		<p></p>
	</>
}
```

这样子在html结构中是不会渲染出`<></>`此时就实现了多个根节点

### 条件渲染

使用分支控制

```rust
html! {
	<>
		<h1 class="title">{ "Hello, world!" }</h1>
		if true {
			<h1 class={myClass}>{ b }</h1>
		} else {
			<h1 class="title">{ "Hello, world! else" }</h1>
		}
	</>
}
```

使用模式匹配（所有的模式匹配在此都可以使用），利用模式匹配和枚举可以实现一些切换表单等

```rust
let mut a = Some(1);
html! {
	<>
		<h1 class="title">{ "Hello, world!" }</h1>
		if let Some(b) = a {
			<h1 class={myClass}>{ b }</h1>
		} else {
			<h1 class="title">{ "Hello, world! else" }</h1>
		}
	</>
}
```

