# less

less是一门css的预处理语言（增强版）

使用更少的代码实现强大的样式

## 样式嵌套

子代的css选择器可以直接卸载父元素的css选择器中

```less
div {
	background-color: #bfa;
	.son {
		background-color: red;
	}
}
```

## 变量

再变量中可以存储**任意值**，再需要时任意修改变量，使用变量

```less
@a: 100px;   /* @变量名: 值 */
@b: box;   /* 存储名字 */
@c: 0;

div {
	width: @a;   /* 使用 */
	@c: 100px;  /* 修改 */
}

.@{b} {}  /* .box {} 作为类名或者一部分值（路径）使用时需要加括号 */  
```

在属性中`$属性名`可以直接引用当前该属性的值

```less
div {
	width: 100px;
	height: $width;
}
```

## 父元素选择器

```less
div {
	& .son {  /* & 就表示外层的选择器 即 div */
	}
}
```

## 扩展

一个选择器在另一个选择器的基础上扩展新的属性

```less
.p2 {}

.p1:extend(.p1) {}   /*在p1的样式上扩展*/
```

## 混合

直接混合其他样式（这样相当于复制，性能较低）

```less
.p2 {}

.pq {
	.p2;
}
```

使用类选择器时添加一个括号代表创建一个`mixins`专门用于给其他选择器引用（不会出现在编译后的css中）

```less
.p2() {} /* 混合函数 */ 

.p1 {
	.p2;
}
```

混合函数可以接收参数（`@`变量），一般用于接收参数来生成不同的样式

```less
.test(@w, @h, @bg-color: red) {
	width: @w;
	height: @h;
	backgroud-color: @bg-color;
}

div {
	.test(200px, 300px);
}
```

这样相比扩展要更灵活，less有很多的内置的混合函数