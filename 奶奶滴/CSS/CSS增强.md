## 伪类选择器

伪类选择器使用`:伪类`形式，例如`:hover`，用于选择元素的特殊状态

伪类用于表示一些特殊的状态

### 常用

- `:hover` 鼠标移动到该元素
- `:active` 鼠标点击


### a标签的伪类选择器

`:link` 用于表示没访问过的链接（正常连接）

```css
a:link {
	color: red;
}
```

`:visited` 用于表示访问过的链接，由于隐私原因只能修改颜色

```css
a:visited {
	color: red;
}
```

## 伪元素

采用两个冒号`::伪元素`，例如`::before`

伪元素表示页面中一些特殊的**并不真实存在**的位置

### 基本使用

`::first-letter` 第一个字母

```css
/* 首字母放大 */
p::first-letter { 
	font-size: 50px; 
}
```

`::first-line` 表示第一行

```css
p::first-line {
	color: red;
}
```

`::selection` 表示选中的内容

```css
p::selection {
	background-color: red;
}
```

**`::before` 表示元素最开始  `::after` 元素最末尾**

由于这两个位置没有任何内容，所有这两个元素必须结合`content`属性一起使用（通过css添加内容无法选中）

```css
div::before {
	content: 'aaa',
	color: red;
}

div::after {
	content: 'bbb',
	color: blue;
}
```

## 继承

元素设置的样式，被元素的后代应用

背景相关、布局相关等不会被继承

`继承 < 通配符选择器 < 类选择器 < id选择器 < 行内样式 < !important`

![权重叠加计算](http://www.droliz.cn/markdown_img/权重计算.jpg)

## 单位

### 长度单位

- 像素（`px`）
- 百分比（`%`）按父元素为`100%`来改变
- `vh、vw` 窗口（页面进去可以看见的视口大小）的高度和宽度，数值相当于百分比
- `em` 是相对于自身字体的大小来的，`1em = 1 font-size`
- `rem` 与`em`相同（`rem -> root em`），不过是相对于根元素字体大小

### 颜色单位

可以采用十六进制`#fff = #ffffff`，也可以采用`rgb(0, 0, 0)`或者添加透明度`rgba(0, 0, 0, .5)`

`rgb`数值可以采用百分比也可以使用`0-225`数值，透明度范围`0-1`小数点`0`可以省略

也可以直接使用颜色名`red`

除了上述还有`hsl`和`hsla`代表：色相（0-360）、饱和度（0-100%）、亮度（0-100%）`hsl(0, 100%, 100%)`

具体按照色相环

## box-sizing

用于决定盒子的可见计算方式（height和width的作用范围）

`content-box`：默认值，用宽高计算内容区大小
`border-box`：宽高设置整个盒子大小（width和height指的是内外边距加上边框加上内容总和）

## 轮廓

`outline` 作用和使用同边框，但是边框会占据盒子的大小，而轮廓不会占据盒子的大小


## BFC

**何为BFC**

   BFC（Block Formatting Context）格式化上下文，是Web页面中盒模型布局的CSS渲染模式，指一个独立的渲染区域或者说是一个隔离的独立容器。

**形成BFC的条件**

  - 1、浮动元素，float 除 none 以外的值；   
  - 2、定位元素，position（absolute，fixed）；   
  - 3、display 为以下其中之一的值 inline-block，table-cell，table-caption；   
  - 4、overflow 除了 visible 以外的值（hidden，auto，scroll）；

**BFC的特性**

- 1、内部的Box会在垂直方向上一个接一个的放置。  
- 2、垂直方向上的距离由margin决定  
- 3、bfc的区域不会与float的元素区域重叠。  
- 4、计算bfc的高度时，浮动元素也参与计算  
- 5、bfc就是页面上的一个独立容器，容器里面的子元素不会影响外面元素。


## 图标字体

由于字体相对图片小很多，所以对于有些小型图标，可以使用将图标设计成字体来实现，提高性能 

可以自己制作，也可以使用网上的免费图标字体库

可以当作一个字体来设置样式

当然，也可以利用伪元素，在元素前面添加图标字体`content: '\编码'`，但是一定要记住需要设置`font-family`指定字体格式

## 渐变

渐变属于图片，很多属性与图片相关，可以使用`background-image`也可以使用`background`

### 线性渐变

默认是从`top`到`bottom`

```css
.div {
	background-image: linear-gradient(direction, color-stop1, color-stop2, ...);
	/* 例如 从左上角到右下角，从红色到蓝色*/
	background: linear-gradient(to bottom right, red, blue);
	/* 也可以直接指定角度 */
	/* 颜色可以选择多个，代表一个过度 */
}
```

每个颜色后面可以带上长度，可以是百分号，也可以是带单位的数值（**都是相对于上述的方向而言**）。表示颜色的起始位置，两个颜色起始位置之间是过渡（渐变效果）

![](https://www.runoob.com/wp-content/uploads/2014/07/7B0CC41A-86DC-4E1B-8A69-A410E6764B91.jpg)

可以设置多个，用英文逗号隔开

```css
.div {
	width: 200px;  
	height: 200px;  
	background: linear-gradient(217deg, rgba(255, 0, 0, .8), rgba(0, 0, 0, 0) 70.71%),  
	linear-gradient(127deg, rgba(0, 255, 0, .8), rgba(0, 0, 0, 0) 70.71%),  
	linear-gradient(336deg, rgba(0, 0, 255, .8), rgba(0, 0, 0, 0) 70.71%);
}
```


除了上述的`linear-gradient`以外，还有`repeating-linear-gradient`，当**渐变效果没有充满元素**时，就会将渐变效果重复，直到铺满元素

```css
.div {  
    width: 200px;  
    height: 200px;  
    background: linear-gradient(red 50px, yellow 100px);  
}
```

上述元素宽高为200px，但是渐变范围只是`50px-100px`也就是`50px`范围，并没有填充元素，那么就会重复填充

此时设置`background-repeat: no-repeat;`是无效的


### 径向渐变

从圆心向四周发散，默认按照元素的形状改变（椭圆、圆）

```css
div {
	height: 200px;
	width: 200px;
	background: radial-gradient(#e66465, #9198e5);
}
```

可以指定渐变范围以及位置

```css
div {
	/* 范围 100% 100% 渐变圆心 0 0 左上角 */
	background: radial-gradient(100% 100% at 0 0,#e66465, #9198e5);
}
```

同样的径向渐变也可以指定多个

渐变范围的取值除了百分比和数值，还可以取以下

|取值|效果|
|:--:|:--|
|`circle`|圆形|
|`ellipse`|椭圆|
|`closest-side`|离圆心最近的边的距离|
|`closest-corner`|离圆心最近的角的边距离作为两个轴|
|`farthest-side`|取远边|
|`farthest-gradient`|取远角|



## 过渡
* 作用：让元素样式慢慢变化
* 属性名：`transition`
* 常见取值

|   参数   |                               取值                                |
| :------: | :---------------------------------------------------------------: |
| 过渡属性 | all：所有能过度的属性都过度、具体属性名如：width：只过度width属性 |
| 过渡时长 |                             数字 + s                              |

基本上可计算的值都可以过渡（宽高、颜色、内外边距等），这些数值必须是有效数值，不能是`auto`

注意点
* 过渡需要：默认状态和hover状态样式不同
* transition是加给需要过渡的元素本身添加的
* transition属性设置在不同状态中效果不同
	* 给默认状态设置，鼠标移入移出都有过渡效果
	* 给hover状态设置，鼠标移入有过渡效果，移出没有

```html
<style>
* {  
    /*font-size: 60px;*/  
    margin: 0;  
    padding: 0;  
}  
  
.box1 {  
    width: 300px;  
    height: 300px;  
    background-color: silver;  
}  
  
.box1 div {  
    width: 100px;  
    height: 100px;  
}  
  
.box2 {  
    background-color: #bfa;  
    transition: all 2s;   /* 所有的可过度属性  在两秒内过度 */
}  
  
.box1:hover .box2 {  
    width: 200px;  
    height: 200px;  
}
</style>

<div class="box1">  
    <div class="box2"></div>  
</div>
```

除了上述的简写，还有以下几个属性（至少要指定属性和时间）

`transition-property` 指定要执行过度的属性 

```css
div {
	transition-property: width, heigth;  /* all */
}
```

`transition-duration` 指定过渡效果持续时间 

```css
div {
	transition-duration: 2000ms;  /* 2s */
}
```

`transition-timing-function` 过渡的时序函数，指定动画过度方式（贝塞尔曲线）

|可选值|说明|
|:--:|:--:|
|`ease`|默认值，慢速开始先加速后减速|
|`linear`|匀速|
|`ease-in`|加速|
|`ease-out`|减速|
|`ease-in-out`|先加后减|

```css
div {
	transition-timing-function: ease;  
}
```

可以使用函数`cubic-bezier()`（贝塞尔曲线函数，通过两个点来确定曲线）自定义

```css
div {
	transition-timing-function: cubic-bezier(.25,.1,.25,1);    
}
```

还可以使用发布执行`steps()`来指定这个过程分为几步，以及执行的时机

```css
div {
	/* 分两步执行，每一步时间 time/2 在开始就执行  默认为end，需要等待time/2才会执行 */  
	transition-timing-function: steps(2, start);    
}
```

`transition-delay` 过渡效果的延迟，等待一段时间后再执行过渡

```css
div {
	transition-delay: 2s;    
}
```

## 动画

动画和过渡类似，但是动画可以自动触发

想要实现动画效果，必须要设置一个关键帧，关键帧设置动画执行的每一个 步骤

使用关键字`@keyframes`定义关键帧

```css
@keyframes Name {
	to { /* 代表起始 */
		margin-left: 0;	
	}
	from {  /* 结束 */
		margin-left: 700px;
	}
}
```

除了使用简单的使用 `to` 和 `from` 还可以使用百分比来表示位置

```css
@keyframes Name {
	0 { 
		margin-left: 0;	
	}
	20% {  
		margin-left: 700px;
	}
	100% {
		margin-left: 800px;
	}
}
```

在使用时需要通过`animation-name`指定动画的关键帧。过渡的那些属性动画也有不过命名由`animation`开头

基本使用

```css
@keyframes Name {
	to { 
		margin-left: 0;	
	}
	from {
		margin-left: 700px;
	}
}

.animation {  
    background-color: #bfa;  
    animation-name: Name;  
    animation-duration: 2s;  
    animation-delay: 2s;  
    animation-timing-function: ease-in-out;  
}
```

除了基本的类似过渡的属性，还有一些其他的属性

`animation-iteration-count` 动画的执行次数默认值为 `1`

```css
div {
	animation-iteration-count: infinite;   /* infinite 代表无数次 */
}
```

`animation-direction` 动画执行方向默认`normal`从 `from` 向 `to`

|参数|说明|
|:--:|:--:|
|`normal`|默认值从 from 到 to|
|`reverse`|从 to 到 from|
|`alternate`|从 from 到 to 但是重复执行时反向|
|`alternate-reverse`|从 to 到 from 但是重复执行时反向|

`animation-play-state` 动画的执行状态默认为 `running`

```css
div {
	animation-play-state: paused;  /* 动画暂停 */
}
```

`animation-fill-mode` 动画的填充模式

```css
div {
	animation-fill-mode: none;   /* 默认，执行完毕回到原来位置 */
	animation-fill-mode: forwards;  /* 执行完毕停在结束位置 */
	animation-fill-mode: backwards;  /* 延时等待是元素就会处于开始位置 */
	animation-fill-mode: both;  /* 结合了 forwards 和 backwards */
}
```

**利用精灵图制作一个动画效果**

![](../../markdown_img/1_WhDjw8GiV5o0flBXM4LXEw.png)

```css
.box1 {  
    height: 256px;  
    width: 256px;  
    margin: 0 auto;  
    background-image: url("./img/1_WhDjw8GiV5o0flBXM4LXEw.png");  
    animation: run 600ms steps(6) infinite;  
}  
  
@keyframes run {  
    from {  
        background-position: 0, 0;  
    }  
    to {  
        background-position: -1536px, 0;  
    }  
}
```

另一个

![](../../markdown_img/Pasted%20image%2020221111200135.png)

```css
.box1 {  
    height: 134px;  
    width: 133px;  
    margin: 0 auto;  
    background-image: url("./img/gaming_DinoSprites_walk.png");  
    animation: run 300ms steps(6) infinite;  
}  
  
@keyframes run {  
    from {  
        background-position: 0, 0;  
    }  
    to {  
        background-position: -800px, 0;  
    }  
}
```

## 变形和旋转

### 变形

变形：通过CSS改变元素的形状或位置（不会影响页面布局）

变形的原点，默认为中心点

```css
div {
	transform-origin: 0 0;
}
```

变形默认是2d变形需要设置`style`才会考虑3d变形

```css
div {
	transform-style: preserve-3d;
}
```

`transform` 用于设置元素的变形效果（z正方向指向屏幕向外）

```css
div {
	transform: translateX(100px);   /*百分比相对于自身*/
	transform: translateY(100px);
	transform: translateZ(100px);
	transform: translate(X, Y);  /*只能设置xy*/
	transform: translateX(100px) translateY(100px) translateZ(100px); 多个
}
```

需要注意的是，z轴属于一个透视效果（近大远小），浏览器默认不开启透视，如果需要必须设置视距即人眼到屏幕的距离

```css
html {
	perspective: 800px;
}

.box1 {  
    width: 200px;  
    height: 200px;  
    background-color: aqua;  
    margin: 200px auto;  
}  
  
body:hover .box1 {  
    transform: translateZ(200px);  
}
```

### 旋转

是元素按沿着 x、y、z旋转指定角度

属性`rotateX、Y、Z`

```css
div {
	transform: rotateZ(45deg);
	backface-visibility: hidden;   /* 是否显示背面 */
}
```

### 缩放

`scaleX、Y` 对元素进行变大缩小

```css
div {
	transform: scale(1.1, 1.1);   /* 双方向 简写 scale(1.1)*/
	transform: scaleX(1.1);
	transform: scaleY(1.1);
}
```





## 变量

CSS是支持变量的（兼容性较差）

变量的定义

```css
* {
	--varName: #bfa;    /* --变量名: 值 */
}
```

使用

```css
html {  
    --color: #bfa;  
    --length: 200px;  
}  
  
div {  
    width: var(--length);  
    height: var(--length);  
    background-color: var(--color);  
    margin: 100px auto;  
}
```

