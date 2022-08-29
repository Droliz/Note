# two.js

## 绘制

### 画布

Two.js的入口：实例化 `Two()` 对象

```js
let elem = document.getElementById('box');  
let params = {    
    // domElement  要绘制的画布或 SVG 元素，会覆盖 type 参数  
    width: 1000,
    height: 1000,
	// 自适应
	fullscreen: true,
	// 设置为true添加要绘制的实例requestAnimationFrame
    autostart: true,    
    type: 'CanvasRenderer'   // 可以是 SvgRenderer
};  
// 创建 Two() 实例对象（画布）
let _two = new Two(params).appendTo(elem);
```

使用 `appendTo(DOM元素)` 的方式附加到 DOM 元素上

### makeLine

根据两个点绘制线段

语法：

```js
Two().makeLine(x1, y1, x2, y2);
```

示例：

```js
// 使用_two.makeLine绘制一条
const _line = _two.makeLine(0, 0, 400, 400);
_line.stroke = '#111';   // 设置线的颜色
_line.fill = 'red';      // 填充颜色
_line.linewidth = 10;    // 线宽
_line.opacity = 0.5;     // 透明度   范围 0-1
_line.visible = true;    // 是否显示 true 显示 false 隐藏
_line.cap = 'round';     // 端点形状  butt 直角  round 圆角  square 方形
_line.id = 'line';       // id
_line.scale = 1;         // 缩放比例
_line.beginning = 0      // 0-1 数值越大,越靠中间
_line.clip = false;      // 是否裁剪 true 裁剪 false 不裁剪
_line.closed = false;    // 是否闭合 false 不闭合 true 闭合
_line.curved = true;     // true 曲线 false 直线
_line.dashes = [10, 30]; // 虚线 分别是线的长度和空隙的长度
_line.rotation = Math.pi / 180;     // 旋转，单位弧度
_line.className = 'line' // 类
```

### makeText

渲染文本

语法

```js
Two().makeText(text, x, y, style);
```

示例

```js
let font = { size: 40, weight: 'normal', family: 'SimHei', fill: '#000' };
const _text = _two.makeText('Hello World', 200, 100, font);
_text.fill = '#fff';
_text.stroke = '#111';
_text.linewidth = 1;
_text.baseline = 'middle'; 
// baseline 设置文字的基线 'top' 'middle' 'bottom'
_text.align = 'right';  // align 设置文字的对齐 'left' 'center' 'right'
_text.id = 'text';
```

### makeGroup

创建组

语法

```js
new Two.Group( 绘制的示例对象 )  // 这种方式创建的组需要添加到要绘制画布 Two() 实例中

Two.makeGroup()  // 添加后需要更新
```

示例：

```js
// 绘制线段
const _line = _two.makeLine(0, 0, 400, 400);
const _line1 = _two.makeLine(400, 400, 500, 400);

// 成组
const _group = _two.makeGroup( _line1);
// 命名组
_group.name = 'group';
// 向组中添加元素
_group.add(_line);
_two.update();


// 直接创建对象
const _group = new Two.Group();
// 添加到绘制的对象中
_two.add(_group);
```


### 添加事件

```js
// 必须在update更新之后再能获取到 DOM 元素
var d = text._renderer.elem;  
// 整个画布DOM Two().renderer.domElement;

// 添加事件
d.addEventListener('click', () => {
    console.log('click');
})

// 也可以采用 jQuery 的用法
var $d = $(text._renderer.elem);
$d.on('click', () => {
	console.log('click');
})
```