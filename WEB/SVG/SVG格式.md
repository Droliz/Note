# SVG格式

SVG属于`xml`结构的文件，并不是二进制文件，SVG可以直接引入到html中进行使用，也可以借助`img、object`等标签引入，采用img引入是无法对SVG进行操作的。

## 标签

SVG文件基本格式如下

```xml
<svg width="100%" height="100%">
	<circle id="mycircle" cx="50" cy="50" r="50" />
</svg>
```

SVG标签如果不指定宽高，默认是 $300px(宽)\ *\ 150px(高)$ 。

## viewBox属性

viewBox属性可以指定SVG的显示区域（实现只显示一部分）

viewBox只有四个属性值 `viewBox="105 55 60 60"`

```text
105 表示相对于svg左上角的横坐标。
55 表示相对于svg左上角的纵坐标。
60 表示截取的视区的宽度。
60 表示截取的视区的高度。
```

最终会将截取的这个视口平铺svg容器

```xml
<svg width="100%" height="100%" viewBox="50 50 50 50">
	<circle id="mycircle" cx="50" cy="50" r="50" />
</svg>
```

如果没有指定svg的宽高，但是指定了viewBox属性。相当于给定长宽比，大小默认等于父元素的大小

