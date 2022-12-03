# Flex布局


## 什么是Flex布局

Flex是Flexible Box的缩写 flex布局表示弹性布局，可以为盒状模型提供最大的灵活性。

**任何一种元素都可以使用Flex布局**

```css
div {
	display: flex;
	/* display: inline-flex; */   /* 行内元素 */
}
```

需要注意的是如果是`webkit`内核的浏览器，必须加上`-webkit`前缀

```css
div {
	display: -webkit-flex;
}
```

## 容器和项目

采用flex布局的元素被称作容器，在flex布局中的子元素被称作项目

即父级元素采用flex布局，则父级元素为容器，全部子元素自动成为项目

项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。

![](../../markdown_img/Pasted%20image%2020221024113135.png)

## 容器属性

### flex-direction属性

决定主轴的方向

```css
.box { 
	flex-direction: row | row-reverse | column | column-reverse 
}
```

默认是`row`即按从左往右

### flex-wrap属性

决定项目排列方式，默认是在一行排列

```css
.box {
	flex-wrap: nowrap | wrap | wrap-reverse
}
```

`wrap`换行，`wrap-reverse`换行且新一行在上方

### flex-flow属性

是`flex-direction`和`flex-wrap`的简写，默认为`row nowrap`

```css
.box {
	flex-flow: <flex-direction> || <flex-wrap>
}
```

### justify-content属性

定义项目在**主轴**上的对齐方式，默认为`flex-start`

```css
.box {
	justify-content: flex-start | center | flex-end | space-between | space-around;
}
```

`space-between`两端对齐，项目之间的间隔都相等

`space-around`每个项目两次之间的间隔相等，所以项目之间的间隔比项目与边框的间隔大一倍

### align-items属性

定义项目在**交叉轴**上如何对齐，默认为`stretch`

```js
.box {
	align-items: flex-start | center | flex-end | baseline | stretch;
}
```

### align-content属性

定义多根轴线（多行）的对齐方式。如果只有一根轴线，不起作用

```css
.box {
	align-content: flex-start | center | flex-end | space-between | space-around | stretch;
}
```

## 项目属性

### order

定义项目的排列顺序，值越小，越靠前，默认为 0 允许有负数

```css
.item {
	order: <number>
}
```


### flex-grow

定义项目的放大比例，默认为 0 ，如果有剩余空间，如果设置了的项目会占据

```css
.item {
	flex-grow: <number>
}
```

### flex-shrink

定义项目缩小比例，如果空间不够，会缩小该项目，默认为 1 （默认就是缩小的）不支持负值

```css
.item {
	flex-shrink: <number>
}
```

### flex-basis

定义在分配多余空间之前，项目占据的主轴空间（`main size`），默认值为`auto`即项目本来的`width`，浏览器根据这个属性判断是否有多余空间

```css
.item {
	flex-basis: <length>
}
```

### flex

`flex-grow、flex-shrink、flex-basis` 的缩写


### align-self

允许单个项目与其他项目有不同的对齐方式，会覆盖`align-items`属性。默认为`auto`表示继承父元素的`align-items`属性，如果没有父元素，默认为`stretch`

```css
.item {
	align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```