# HTML5

## 实体

在编写网页中，多个空格会被浏览器解析为一个空格，那么如果需要多个空格可以采用实体替换

| 显示结果 |  描述  |      实体名称       |
| :------: | :----: | :-----------------: |
|  &nbsp;  |  空格  |       \&nbsp;       |
|   &lt;   | 小于号 |        \&lt;        |
|   &gt;   | 大于号 |        \&gt;        |
|  &amp;   |  和号  |       \&amp;        |
|  &quot;  |  引号  |       \&quot;       |
|  &apos;  |  撇号  | \&apos;（IE不支持） |
|  &cent;  |   分   |       \&cent;       |
| &pound;  |   镑   |      \&pound;       |
|  &yen;   |   元   |       \&yen;        |
|  &euro;  |  欧元  |       \&euro;       |
|  &sect;  |  小节  |       \&sect;       |
|  &copy;  |  版权  |       \&copy;       |


## meta标签

用于设置网页元数据，表示不能被其他源标签表示的数据`link、script、title、base、style`等

使用`keywords`指定关键字

```html
<meta name="keywords" content="关键字1,关键字2,关键字3">
```

使用`description`指定网站描述

```html
<meta name="description" content="描述">
```

指定网站编码

```html
<meta charset="UTF-8">
```

重定向

```html
<!-- 三秒后跳转到 baidu -->
<meta http-equiv="refresh" content="3,url=http://www.baidu.com">
```

## 语义化标签

指一些由特殊代表的标签，例如`title、header、body`等有特殊代表的标签

## 标签

在标题标签中可以将有关联的标签放到`hgroup`标签中，对标签进行一个组合

`<em>` 标签是一个短语标签，用来呈现为被强调的文本。

`<strong>`定义重要的文本。

`<dfn>`定义一个定义项目。

`<code>`定义计算机代码文本。

`<samp>`定义样本文本。

`<kbd>`定义键盘文本。它表示文本是从键盘上键入的。它经常用在与计算机相关的文档或手册中。

`<var>`定义变量。您可以将此标签与 `<pre>` 及 `<code>` 标签配合使用。

`<q>`短引用，会带上双引号

`<blockquote>`长引用，会向后缩进

具体参考[菜鸟教程](https://www.runoob.com/tags/tag-comment.html)

## 超链接

H5中可以通过`href="#top"`实现回到顶部的作用，通过 `href="#id"`去到指定id的标签

