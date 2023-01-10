# CSS

> ***CSS（层叠样式表）:给HTML标签美化***

## CSS引入方式
### 内嵌式

CSS写在 ***style标签*** 中，style标签一般写在head标签里面，title标签下面

```css
选择器(标签名) {
		属性更改
	}

<style>
	/* 更改 p 标签的样式 */
	p {
		color: red;
		font-size: 20px;
	}
</style>
```

### 外联式

CSS写在单独的CSS文件中，通过link标签（写在head标签中）在网页中引入

例如:`<link rel="stylesheet" href="CSS文件">`

>rel:文件和html的关系（stylesheet:样式表）

### 行内式

CSS写在 ***style属性*** 中，配合js使用

例如: `<p style="color: green"; font-size: 20px;>文本文档</p>`

| 引入方式 |          书写位置           | 作用范围 |  使用场景  |
| :------: | :-------------------------: | :------: | :--------: |
|  内嵌式  |     style ***标签*** 中     | 当前页面 |   小案例   |
|  外联式  | CSS文件中，通过link标签引入 | 多个页面 |    项目    |
| 行内式式 |     style ***属性*** 中     | 当前页面 | 配合js使用 |


## CSS基础

---
### 选择器

#### 标签选择器

结构：`标签名{CSS属性名: 属性值;}`

作用：通过标签名，找到页面中 ***所有*** 这类标签，设置样式

注意点:
* 标签选择器选择的是所有这个标签的
* 无论嵌套多少，都能找到这个标签，并更改
  
示例:
```html
<style>
	p{
		color: aqua;
	}
</style>

<p>文本文档</p>
```

### 类选择器

结构：`类名{CSS属性名:属性值;}` 

作用：通过类名，更改所有包含这个类名属性的标签

注意点:
* 所有标签都有class属性，属性值称为类名
* 类名不能以数字或中划线开头
* 一个标签可以有多个类名
* 类名可以重复，一个类选择器可以同时选中多个标签

示例:

```html
<style>
	/* 定义类 */
	.one{
		color: red;
	}
	.tow{
		font-size: 12px;
	}
</style>
/* 使用类 */
<p class="one">文本文档</p>
<p class="one tow">文本文档</p>
```

### id选择器
结构：`#id属性值{CSS属性名:属性值;}`

作用：通过id属性值，找到页面带有这个i的属性值的标签

注意点:
* 所有标签都有id属性
* id属性类似身份证号码，在一个页面中是唯一的，***不可重复***
* 一个标签只能有一个id
* 一个id选择器只能选择一个标签

>配合 js 找标签

示例:

```html
<style>
	#one{
		color: red;
	}
</style>

<p id="one">文本文档</p>
```

### 通配符选择器

结构：`*{CSS属性名: 属性值;}`

作用：选择页面所有标签

注意点:
* 极少用到
* 一般使用时都是使用margain、padding(清除标签的原始内外边距)两个属性
  
示例:

```html
<style>
	*{
		margin: 0;
		padding: 0;
	}
</style>

<p>文本文档</p>
```

###  复合选择器

**后代选择器**

* 作用:根据HTML标签的嵌套关系，选择父元素后代中满足条件的
* 选择器语法:选择器1 选择器2{CSS}
* 结果:在选择器1中所有后代（子代、孙代、重孙等）满足选择器2的标签
* ***选择器与选择器之间空格隔开***

示例:

```html
/* 只会更改div中的p */ 
<style>
	div p{
		color: white;
	}
</style>

<p></p>
<div>
	<p></p>
</div>
```
   
**子代选择器**

* 在后代选择器基础上只选择子代
* 语法: 选择器1 > 选择器2{CSS}

**并集选择器**

* 作用:同时选择多组标签
* 语法: 选择器1 , 选择器2 , 选择器3{CSS}
* 找到选择器1和选择器2中的所有标签
* 选择器可以是基础选择器也可以是后代选择器

**交集选择器**

* 作用:找到满足两个选择器的所有标签
* 语法:选择器1选择器2{CSS}
* ***选择器之间不加任何东西***

**hover伪装选择器**

* 作用:选中鼠标 **悬停** 在元素上的 **状态**
* 语法:选择器:hover{CSS}
* 注意点:
	* 伪类选择器选中的颜色的 ***某种状态***

示例:
```html
<style>
	a: hover{
		color: red;
		background-color: #808080;
	}
</style>

<a href="#">超链接</a>
```

**深度（穿透）选择器**

`>>>选择器{css}`

深度选择器可以让该选择器下的所有都应用

```css
<style>
    >>> .el-carousel__button{
		width:10px;
		height:10px;
		background:red;
		border-radius:50%;
	}
</style>
```

### 字体和文本样式

**字体**

字体属于矢量，不会失真

字体大小:
* 属性名:font-size
* 取值:数字 + px
* 注意点:
	* 谷歌浏览器默认文字大小是 16px
	* 单位需要设置，否则无效

示例:

```html
<style>
	p{
		font-size: 20px;
	}
</style>

<p>文本文档</p>
```

字体粗细:
* 属性名:font-weight

| 样式  | 属性名 |
| :---: | :----: |
| 正常  | normal |
| 加粗  |  bold  |

| 样式  | 属性名 |
| :---: | :----: |
| 正常  |  400   |
| 加粗  |  700   |

注意点:

* 不是所有字体都提供了九种粗细
* 正常、加粗两种取值更多

示例:

```html
<style>
	p{
		font-weight: normal;
	}
	*{
		font-weight: 700;
	}
</style>

<p>文本文档</p>
```

字体样式:

* 属性名:font-style

| 样式  | 属性名 |
| :---: | :----: |
| 正常  | normal |
| 倾斜  | italic |

示例:

```html
<style>
	p{
		font-style: italic;
	}
</style>

<p>文本文档</p>
```

字体类型:

属性名:font-family
* 具体字体:"Microsoft YaHei"、微软雅黑、黑体等
* 字体系列:sans-serif（无衬线字体）、serif（衬线字体）、monospace（等宽字体）等

渲染规则:
* 从左往右按照顺序查找，如果电脑未安装该字体，则显示下一个
* 如果都不支持，根据操作系统显示最后字体系列的默认字体

注意点:
* 如果字体名称存在多个单词，推荐使用引号包裹
* 最后一项 ***字体系列不需要引号包裹***
* 尽量使用系统常见字体

示例:

```html
<style>
	p{
		font-style: 微软雅黑, 黑体, sans-serif;
	}
</style>

<p>文本文档</p>
```

>样式层叠：同一标签的同一个样式属性重复多写，后面的会覆盖前面的，写在最后面的生效

font(复合属性)

示例:

```html
<style>
	p{
		/* 空格隔开 */
		font: 700 66px 宋体 italic;
	}
</style>

<p>文本文档</p>
```   


**文本样式**

文本缩进

* 属性名:text-indent 
* 取值:
	* 数字 + px
	* 数字 + em（推荐 1 em = 当前标签的font-size的大小）

`首行缩进: text-indent: 2em;`

文本水平对齐方式

* 属性名:text-align

| 属性值 |      效果      |
| :----: | :------------: |
|  left  | 左对齐（默认） |
| center |    居中对齐    |
| right  |     右对齐     |

* 注意点:
	* 如果需要让文本水平居中，text-align属性给文本所在标签（文本父元素）设置

>img、span、input等都有适用

```html
<style>
	/* 例如图片居中需要对他的父元素body居中 */
	body {
		text-align: center;
	}
</style>

<body>
	<img src="">
</body>
```

文本修饰

* 属性名:text-decoration

|    属性值    |   效果   |
| :----------: | :------: |
|  underline   |  下划线  |
| line-through |  删除线  |
|   overline   |  上划线  |
|     none     | 无装饰线 |

* 注意点:
	* none经常用于清除a标签默认的下划线

```html
<style>
	a {
		text-decoration: none
	}
</style>

<a href="http://">超链接</a>
```

css3 中可以在 `@font-face` 中编写引用字体（引用服务器的字体）

```css
/* 名字和路径必选 */
@font-face {
	font-family: 'Font-Name',   /* 字体的名称 */
	src: url('./name.ttf'),     /* 字体文件路径 */
	font-weight: normal,      
	font-style: normal,
	font-stretch: normal
}

/* 需要使用时就直接引用字体名称即可 */
div {
	font-family: 'Font-Name'
}
```

**行高**

作用:控制一行的上下间距

属性名:line-height

取值:
* 数字 + px
* 倍数（当前标签font-size的倍数）

应用:
* 让单行文本垂直居中可以设置`line-height: 文字父元素高度;`
* 网页精准布局时，会使用`line-height: 1;`取消上下行间距

注意点:
* 如果同时设置了行高和font连写，注意覆盖问题
* `font: style weight size/line-height family;`(倾斜加粗可以省略，按顺序)

```html
<style>
p{
	font: italic 700 66px/2 宋体;
}
</style>

<p>文本文档</p>
```

![文本](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimage.uisdc.com%2Fwp-content%2Fuploads%2F2019%2F02%2Fuisdc-wy-20190225-28.jpg&refer=http%3A%2F%2Fimage.uisdc.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1645310591&t=c9b06608840f0ed5434450cf4066287c)


**标签水平居中**

* 通过`margin: 0 auto;`实现 div、p、h（大盒子）等标签居中显示
* 注意点:
    * 直接给当前元素本身设置即可
    * 一般是针对固定宽度的盒子，如果大盒子没有设置宽度，此时会默认占满父元素的宽度

示例:
```html
<style>
div{
	width: 300px;
	height: 300px;
	background-color: pink;
	/* auto：自适应 */
	margin: 0 auto;
}
</style>

<div></div>
    ```


## CSS进阶

### 背景

* 背景颜色
    * background-color: 颜色;
* 背景图
    * background-image: url(路径);
	* 尽量不使用
	* 添加线性渐变

```css
background-image: linear-gradient(direction, color-stop1, color-stop2, ...);
/* 例如 从左上角到右下角，从红色到蓝色*/
background-image: linear-gradient(to bottom right, red, blue);
/* 也可以直接指定角度 */
/* 颜色可以选择多个，代表一个过度 */
```

![](https://www.runoob.com/wp-content/uploads/2014/07/7B0CC41A-86DC-4E1B-8A69-A410E6764B91.jpg)

径向渐变

* `background-image: radial-gradient(shape size at position, start-color, ..., last-color);`

```css
/* 可以选择形状和颜色的占比，不选择默认平均 */
background-image: radial-gradient(circle, red 5%, yellow 15%, green 60%);
```

 不同大小关键字
-   **closest-side**
-   **farthest-side**
-   **closest-corner**
-   **farthest-corner**

```css
background-image: radial-gradient(farthest-side at 60% 55%, red, yellow, black);
```

背景平铺

* background-repeat

|   取值    |          效果          |
| :-------: | :--------------------: |
|  repeat   | （默认）水平垂直都平铺 |
| no-repeat |         不平铺         |
| repeat-x  |        水平平铺        |
| repeat-y  |        垂直平铺        |

平铺：复制直到铺满

背景位置
* background-position: 水平位置 垂直位置;
* 取值：
	* 方位名词
		* 水平: left、center、right
		* 垂直: top、center、bottom
	* 数字 + px
		* 原点在左上角
		* x: 水平向右
		* y: 垂直向下

***背景相关属性和 font 一样也可以连写，但这个不分先后顺序***\
`background: color image repeat position`


### 元素显示模式

#### 块级元素
* 显示特点
	* 独占一行（一行只显示一个）
	* 宽度默认是父元素的宽度，高度和内容相同
	* 可以设置宽高
* 代表标签
	* `div、p、h系列、ul、li、dl、dt、dd、form、header、nav、footer等`
  
#### 行内元素
* 显示特点
	* 一行可以显示多个
	* 宽度和高度默认和内容相同
	* 不可以设置宽高
* 代表标签
	* `a、span、b、u、i、s、strong、ins、em、del等`

行内元素可以设置`margin、padding、border`但是垂直方向不影响

而且水平方向的外边距不会出现合并现象

#### 行内块元素
* 显示特点
	* 一行可以显示多个
	* 可以设置宽高
* 代表标签
	* `input、textarea、button、select等`

#### 元素显示模式的切换

| 属性                  | 效果                  |
| :-------------------- | :-------------------- |
| display: block        | 转换为 **块级元素**   |
| display: inline       | 转换为 **行内元素**   |
| display: inline-block | 转换为 **行内块元素** |
|diaplay: table|设置为表格|
|diaplay: none|隐藏|


`visibility`元素用于设置元素的显示状态默认为`visible`，为`hidden`时隐藏，但是占位


***标签的嵌套***

* 1、块级元素一般作为大容器，可以嵌套：文本、块级元素、行内元素等等

但是：***p标签中不要嵌套div、p、h等块级元素***

* 2、a标签内部可以嵌套任意的元素

但是：***a标签内部不能嵌套a标签***

### CSS特性

#### 继承性
* 特性：子元素有默认继承父元素样式的特点
* 可以继承的常见属性（文字控制类属性都可以继承）
	* `color、font-style、font-weight、font-size、font-align、line-height等`
* 可以通过调试工具判断属性是否可以继承

#### 层叠性
* 特性
	* 给同一个标签设置 ***不同*** 的样式，此时样式会层层叠加，共同作用在标签上
	* 给同一个标签设置 ***相同*** 的样式，姿势样式会层层叠加，最终写在最后的样式会生效
* 当样式冲突时，只有当选择器优先级相同时，才能通过层叠性判断结果

#### 优先级
* 特性：不同选择器具有的优先级不同，优先级高的会覆盖优先级低的
* 优先级公式
	* 继承 < 通配符选择器 < 类选择器 < id选择器 < 行内样式 < !important
	* !important写在属性值的后面，分号的前面
	* !important不能提升继承的优先级
* 叠加计算
	* 权重叠加计算
		* 场景：如果是复合选择器，此时需要通过权重叠加计算方法，判断最终那个选择器优先级最高会生效
		* 权重叠加计算公式：（每一级之间不存在进位）
		* 一级一级比较，当比较有结果，后面都不管
		* 如果所有数字都相同，表示优先级相同，比较层叠性。
		**如果!important不是继承，则权重最高**
		**如果都是继承，那么比较继承的父级权重**

![权重叠加计算](http://www.droliz.cn/markdown_img/权重计算.jpg)


## CSS盒子模型 

### 介绍

  * 概念
      * 页面中的每一个标签都可以看作是一个盒子，更方便布局
      * 浏览器在渲染网页时，会将网页元素看作是一个个矩形（盒子）
  * CSS中规定每个盒子分别由：内容区域（content）、内边框区域（padding）、边框区域（border）、外边距区域（margin）构成

![盒子模型](http://www.droliz.cn/markdown_img/盒子模型.jpg)

示例:

```html
<style>
  div{
	  width: 300px;
	  height: 300px;
	  background-color: pink;
	  border: 1px solid #000;
	  padding: 20px;
	  margin: 50px;
  }
</style>

<div>div标签</div>
```

### 使用

* 内容
    * 内容的width、height默认默认设置是盒子**内容区域**的大小

* 边框 - 连写模式（border）
    * `border: 1px solid #000;（粗细、线条种类、颜色）`不分先后顺序
    * 如果要单独设置某个方向:`border-left: 1px solid #000;（border-方向名词）`

* 内边距（padding）
    * padding: 20px 16px 32px 20px;
    * 可以写多个值，最多四个，代表四个方向的内边距
        * 四个值: 上  右  下  左
        * 三个值: 上  右/左  下
        * 两个值: 上/下  右/左
        * 一个值: 上/下/左/右

>内减模式：在设计尺寸时，给盒子添加属性`box-sizing: border-box;`就可以自动计算多余位置，自动在内容中减去，不用自己计算调整

* 外边距（margin）
    * 同内边距（作用在盒子外，与其他盒子的距离）

* ***清除默认的内外边距***
    * 例如p、h、ul等标签是有默认的内外边距
    
```css
* {
	/* 清除所有默认的内外边距 */
	margin: 0;
	padding: 0;
}
```

* 版心居中
    * 版心：网页的有效内容
    `margin: 0 auto;`

>布局顺序：从外往内，从上往下

***注意***

外边距折叠现象
* ***合并现象***
	* 场景：**垂直布局** 的 **块级元素** 上下的margin会合并
	* 结果：最终两者距离为margin的最大值（一正一负取和，都负取绝对值大的）
	* 解决方法：只给予其中一个盒子设置margin即可
* ***塌陷现象***
	* 场景：**互相嵌套** 的 **块级元素**，子元素的 margin-top会作用在父元素上
	* 结果：导致父元素和子元素一起向下移动
	* 解决方法：
		* 给父元素设置border-top或者padding-top
		* 给父元素设置overflow：hidden
		* 转换成行内块元素
		* 设置浮动

盒子模型中只有`width、margin-left、margin-right`三个属性可以设置`auto`   

**而在元素的水平布局中，子元素的水平方向上的长度和，一定是等于父元素的宽度。如果不够，会将上述可设为`auto`的值加上缺少的值达到等于的效果**

外边距折叠可以使用伪元素解决（同时解决浮动塌陷）

```css
.clearFix::before, .clearFix::after {
	content: '';   
	display: table;   /* 需要独占一行，且table可以同时及解决高度塌陷和外边距重叠 */
	clear: both;   /* 清除浮动 */   
}
```

## CSS浮动


### 结构伪类选择器

* 作用与优势
    * 作用：根据元素再HTML中的结构关系查找元素
    * 优势：减少对于HTML中类的依赖，有利于保持代码的整洁
    * 场景：常用于查找某父级选择器的子元素
* 选择器：

|        选择器         |                   说明                   |
| :-------------------: | :--------------------------------------: |
|    E:first-child{}    |  匹配父元素中第一个子元素，并且是E元素   |
|    E:last-child{}     | 匹配父元素中最后一个子元素，并且是E元素  |
|   E:nth-child(n){}    |   匹配父元素中第n个子元素，并且是E元素   |
| E:nth-last-child(n){} | 匹配父元素中倒数第n个子元素，并且是E元素 |

* n的取值除了自然数还有

|      功能       |      取值       |
| :-------------: | :-------------: |
|      偶数       |    2n、even     |
|      奇数       | 2n+1、2n-1、odd |
|    找到前m个    |      -n+m       |
| 找到从第m个往后 |       n+5       |

>m任意正整数


### 伪元素
* 伪元素：由CSS模拟出的标签效果，一般页面的非主体内容可以使用伪元素，对元素的所有操作，对伪元素也可
* 种类：

|  伪元素  |               作用               |
| :------: | :------------------------------: |
| ::before | 在父元素内容的最前添加一个伪元素 |
| ::after  | 在父元素内容的最后添加一个伪元素 |

* 注意点：
    * 必须设置content属性才能生效
    * 伪元素默认是行内元素(宽高不生效)

```html
<style>
    .father{
        width: 300px;
        height: 300px;
    }

    .father::before{
        content: 内容;
        color: red;
    }

    .father::after{
        content: 内容;
        color: red;
    }
</style>

<div class="father"></div>
```


### 标准流

* 标准流又称文档流，是浏览器在渲染网页内容时默认采用的一套排版规则，规定了应该以何种方式排列元素
* 常见标准流
	* 块级元素：从上到下，垂直布局，独占一行
	* 行内元素 / 行内块元素：从左往右，水平布局，空间不够自动折行（行内元素宽高由内部元素撑开）


### 浮动

>浏览器在解析行内块或者行内标签时，如果标签代码换行会产生一个空格，不利于网页开发

* 作用：图文环绕（早期）网页布局（现在）
* 代码：

```html
<!-- 图文环绕 -->
<style>
	img{
		float: left;
	}
</style>

<img src="http://">
<p>文本文档</p>
```

```html
<!-- 网页布局 -->
<style>
	div{
		width: 300px;
		height: 300px;
	}

	.one{
		background-color: pink;
		float: left;
	}

	.two{
		background-color: aqua;
		float: left;
	}

</style>
<!-- 即便代码换行，在同一行显示并且不会产生空格 -->
<div class="one">one</div>
<div class="two">two</div>
```

* 特点
    * 浮动的标签默认是顶对齐的
    * 浮动的标签会脱离标准流（脱标），在标准流中不占位置
    * 浮动比标准流高半个级别，可以覆盖标准流中的元素
    * 浮动找浮动，下一个浮动元素会跟随上一个浮动元素后面左右浮动
    * 浮动元素有特殊的显示效果
        * 一行显示多个
        * 可以设置宽高

>浮动元素不能通过text-align：center；或者margin：0 auto；

### 清除浮动

`clear: both`

* 介绍
    * 含义：***清除浮动带来的影响***
        * 影响：如果子元素浮动了，此时子元素不能撑开标准流的块级父元素
    * 原因：子元素浮动后脱标 -> 不占位置
    * 目的：需要父元素有高度，从而不影响其他网页布局
* 清除方法：
    * ***设置父元素高度***
    * ***额外标签***
        * 操作：
            * 在父元素内容的最后添加一个块级元素
            * 给添加的块级元素设置 `clear: both;`
        * 特点：会在页面中添加额外的标签，使HTML结构变得复杂

```html
<style>
.top {
	margin: 0 auto;
	width: 1000px;
	background-color: pink;
}
.bottom {
	height: 100px;
	background-color: green;
}
.left_div {
	float: left;

	width: 200px;
	height: 300px;
	background-color: #897045;
}
.right_div {
	float: right;

	width: 790px;
	height: 300px;
	background-color: skyblue;
}

.clear_fix {
	clear: both;
}
</style>

<div class="top">
<div class="left_div"></div>
<div class="right_div"></div>
<div class="clear_fix"></div>
</div>
<div class="bottom"></div>
```

**单伪元素清除法**

* 操作：用伪元素替代额外标签

```css
/* 直接将clear_fix类添加到父元素 */
.clear_fix::after {
	content: "";
	display: block;
	clear: both;
	/* 使页面中看不到伪元素 */
	height: 0;
	visibility: hidden;
}
```

**双伪元素清除法**

* 操作:

```css
/* 直接将clear_fix类添加到父元素 */
.clear_fix::before {}
.clear_fix::after {
	content: '';
	/* 转为表格 */ 
	display: table;
}

.clear_fix::after{
	clear: both;
}
```

**overflow***

* 给父元素设置`overflow: hidden;`属性

## CSS定位装饰

### 定位
#### 基本介绍

可以让元素 ***自由的*** 摆放在网页任意位置，一般用于盒子之间的层叠情况

#### 使用场景

* 解决盒子之间的层叠问题
	* 定位之后的元素层级最高，可以层叠在其他盒子上面

* 让盒子固定在页面的某个位置


#### 使用
* 设置定位方式
	* 属性名：`position`
	* 常见属性值

| 定位方式 |  属性值  |
| :------: | :------: |
| 静态定位 |  static  |
| 相对定位 | relative |
| 绝对定位 | absolute |
| 固定定位 |  fixed   |
|粘滞定位|sticky|

* 设置偏移值
	* 偏移值设置分为两个方向，水平和垂直各选一个
	使用即可（***取值也可以是百分比*** 如果水平和垂直都有两个，以left和top为准）
	* 选取的原则一般是就近原则

| 方向  | 属性名 |  属性值   |      含义      |
| :---: | :----: | :-------: | :------------: |
| 水平  |  left  | 数字 + px | 距离左边的距离 |
| 水平  | right  | 数字 + px | 距离右边的距离 |
| 垂直  |  top   | 数字 + px | 距离上边的距离 |
| 垂直  | bottom | 数字 + px | 距离下边的距离 |

#### 分类
 
**静态定位**
* 所有支持position的元素都默认静态定位

**相对定位**    

* 介绍：相对自己之前的位置进行移动
* `position: relative`
* 特点：
	* 具备原有标签的显示模式
	* 在页面中占有原来的位置 -> **没有脱标**
* 应用场景：
	* 配合绝对定位（子绝父相）
	* 用于小范围的移动

**绝对定位**
* 介绍：相对于非静态的父元素进行定位移动
* `position: absolute`
* 特点：
	* 默认在浏览器可视区域移动
	* 再页面中不占位置 -> **脱标**
	* 改变标签显示模式具备行内块特点
* 应用场景：
	* 配合相对定位

>逐层向外查找父级（有定位就跟随定位父级），如果没有父级或父级没定位，则以浏览器窗口为参照进行定位

居中

```html
<style>
	.center {
		position: absolute;
		top: 50%;
		left: 50%;
		/* 位移，相对盒子的宽高 */
		transform: translate(-50%, -50%);
		
		width: 200px;
		height: 200px;
		background-color: pink;
	}
</style>

<div class="center"></div>
```

或者

```css
/* 利用绝对定位中水平垂直浏览器都会计算 所有水平垂直之和 = 父元素宽高 */
.center {
	position: absolute;
	width: 200px;
	height: 200px;
	background-color: pink;
	top: 0;   /* 去除默认的 aotu 避免浏览器自动填充 */
	right: 0;
	bottom: 0;
	left: 0;
	margin: auto;  /* 浏览器自动填充 */
}
```

**固定定位**
* 介绍：相对于浏览器位置改变
* `position: fixed`
* 特点：
	* 页面中不占位置 -> **脱标**
	* 显示模式具备行内块特点
	* 让盒子固定在页面某个位置

**粘滞定位**

- 和相对定位基本一致
- `position: sticky`
- 粘滞定位可以在元素到达某个位置时在视窗中不动，当滚动到之前的位置，那么就会回到原始位置

粘滞定位不**脱标**

例如：京东的侧边菜单

```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <meta content="关键字1,关键字2,关键字3" name="keywords">  
    <title>Title</title>  
    <style>  
        * {  
            font-size: 60px;  
        }  
  
        .box1 {  
            width: 200px;  
            height: 200px;  
            background-color: #bfa;  
        }  
  
        .box4 {  
			position: sticky;  
            top: 300px;    /* 当box4top达到300px时就会固定住 */
  
            width: 200px;  
            height: 200px;  
            background-color: orange;  
        }  
  
        .box3 {  
            width: 200px;  
            height: 200px;  
            background-color: yellow;  
        }  
  
    </style>  
</head>  
<body>  
<div>  
    <div class="box1">1</div>  
    <div class="box2">2</div>  
    <div class="box3">3</div>  
    <div class="box3">3</div>  
    <div class="box4">4</div>  
    <div class="box3">3</div>  
    <div class="box3">3</div>  
    <div class="box3">3</div>  
</div>  
</body>  
</html>
```

>标准流 < 浮动 < 定位\
>定位中，HTML写在下面的元素级别高，会覆盖上面的元素
>如果想人为干扰定位的级别（结构上同一层的元素），添加`z-index: 数字;`属性,默认位 0 ，数字越大级别越高（前提是元素开启定位）

### 装饰

**垂直对齐**
* 基线（浏览器按基线对齐）
	* 浏览器文字类型元素排版中存在用于对齐的基线

![基线](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Faduroidpc.top%2Fimg%2Ftext-jixain.png&refer=http%3A%2F%2Faduroidpc.top&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1645605160&t=103fdc1f3432b4e06d8191a518f8176c)

**对齐方式**

* 属性名：`vertical-align`
* 属性值：

|  属性值  |       效果       |
| :------: | :--------------: |
| baseline | 基线对齐（默认） |
|   top    |     顶部对齐     |
|  middle  |     中部对齐     |
|  bottom  |     底部对齐     |

**光标类型**
* 场景：设置鼠标在元素上时显示的样式
* 属性名：cursor
* 常见属性值：

| 属性值  |                效果                |
| :-----: | :--------------------------------: |
| default |         通常是箭头（默认）         |
| pointer |     小手效果，提示用户可以点击     |
|  text   | 工字型，提示可以选择文字（复制等） |
|  move   |     十字光标，提示用户可以移动     |

**变框圆角**

* 场景：让盒子四个角变圆润，增加页面细节，提升用户体验
* 属性名：border-radius
* 取值：数字 + px、百分比（圆角的半径）
* 赋值规则：从左上角开始顺时针，没有赋值的和对角相同（与边框相同）

**溢出部分显示效果**

* 溢出部分：盒子的 内容部分 超过了盒子范围的区域
* 场景：控制溢出效果显示（隐藏、显示、滚动条）
* 属性名：overflow
* 属性值：

| 属性值  |               效果                |
| :-----: | :-------------------------------: |
| visible |       溢出部分可见（默认）        |
| hidden  |           溢出部分隐藏            |
| scroll  |     无论是否溢出都显示滚动条      |
|  auto   | 根据是否溢出，自动显示/隐藏滚动条 |

**元素本身隐藏**

场景：让元素本身在屏幕中不可见。如：下拉菜单中的元素

常见属性：

* `visibility: hidden;`（占位隐藏）
* `display: none;`（不占位）

```html
<style>
/* 鼠标悬停标签:hover 要显示的标签 */
div:hover img {
	display: block;
}
</style>
```


## 拓展

### 去掉列表的符号

```css
ul {
	list-style: none;
}
```

### CSS属性书写顺序（浏览器执行效率更高）
* 浮动/display
* 盒子模型：margin、border、padding、宽高背景色
* 文字样式

### `.clear_fix::before {}`（双伪元素清除法）
* 在产生浮动影响和外边距塌陷都可以解决

### 去除元素初始样式

``` css
/* 去除初始样式 */
* {
	margin: 0;
	padding: 0;
	/* 内减模式 */
	box-sizing: border-box;
}
```

### 元素整体透明
* 属性名：opacity
* 属性值：0-1
	* 0：完全透明
	* 1：完全不透明

### 精灵图
* 场景：项目中将多张小图片合成为一张大图片，这张大图片称之为精灵图
* 优点：可以减少服务器发送次数，减轻服务器压力，提高网页加载速度
* 使用精灵图
	* 定义一个和精灵图中需要的图大小相同的标签
	* 设置背景图为精灵图
	* 添加属性：`background-position: -x -y;`
		* x,y分别为此小图的垂直方向位置和水平方向位置（属性值记得取负值）

### 背景图片大小
* 设置背景图片大小
* `background-size: 宽 高;`
* 取值

|   取值    |                              场景                              |
| :-------: | :------------------------------------------------------------: |
| 数字 + px |                            简单方便                            |
|  百分比   |                    相当于盒子自身宽高百分比                    |
|  contain  |          包含，等比例将图片缩放，直到不会超出盒子大小          |
|   cover   | 覆盖，将图片等比例缩放，直到刚好填满整个盒子（没有空白的地方） |

###  background连写

* `background: color image repeat position/size;`
* background-size在连写时需要注意覆盖问题

### 盒子阴影

* 作用：给盒子添加阴影
* 属性名：`box-shadow`
* 取值:（按顺序）

|   参数   |            作用            |
| :------: | :------------------------: |
| h-shadow |  水平偏移允许负值（必须）  |
| v-shadow |  垂直偏移允许负值（必须）  |
|   blur   |       模糊度（可选）       |
|  spread  |      阴影扩大（可选）      |
|  color   |      阴影颜色（可选）      |
|  inset   | 将阴影改为内部阴影（可选） |

