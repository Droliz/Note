
## 元素居中

### 文字居中

- 文本
- `span`，`a`
- `input`，`img`

```less
div {  /* 父元素 */
	text-align: certer;    /* 水平居中 */
	height: 40px;
	span {
		line-height: 40px;   /* 与父元素height一致  垂直居中 */
	}
}
```

vertical-align 的适用元素和text-align一样

取值取中部对其

```less
span {
	vertical-align: middle;
}
```


### 版心居中

不可用于浮动元素

```css
div {
	margin: 0 auto;
}
```


### 绝对定位

通过绝对定位实现元素的垂直水平居中

```css
div {   /* 居中元素 */
    position: absolute;  
    top: 50%;   /* calc(50% - 100px) */
    left: 50%;  
    width: 200px;  
    height: 200px;  
    transform: translate(-50%, -50%);  
    background-color: aqua;  
}
```

如果是如上的已知宽高，那么top和left可以使用计算函数计算`calc(50% - 宽高一半px)`

### 弹性盒子

通过`flex`特性实现，多个项目，则会以项目中心当作中心

```less
.parent {
	display: flex;  
	justify-content: center;  
	align-items: center;
	.son {
		width: 50px;  
		height: 50px;  
		background-color: rgb(240, 248, 166);
	}
}
```

### 网格布局

```less
.parent {  
    background-color: #eee;  
    height: 1000px;  
    width: 1000px;  
    display: grid;  
    align-items: center;  
    justify-items: center;  
	.son {  
	    align-self: center;  
	    justify-self: center;  
	    width: 50px;  
	    height: 50px;  
	    background-color: rgb(240, 248, 166);  
	}
}  
```

## 浮动塌陷与外边距重叠

一般使用类名为`clearfix`

```css
.clearFix::before, 
.clearFix::after {
	content: '';   
	display: table;   /* 需要独占一行，且table可以同时及解决高度塌陷和外边距重叠 */
	clear: both;   /* 清除浮动影响 */   
}
```