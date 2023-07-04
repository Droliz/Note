## 组件中使用css

可以在`html!`宏中直接编辑`class`来实现

```rust
html! {
	<div>
		<h1 class="title">{ "hello world" }</h1>
	</div>
}
```

上述这样仅仅是设置一个类名，具体的样式还是需要另外设置

类似上述的，我们也可以通过变量的形式设置，不过一样的，变量是需要`{}`包裹的

```rust
let myClass = "title";
html! {
	<div>
		<h1 class={ myClass }>{ "hello world" }</h1>
	</div>
}
```

