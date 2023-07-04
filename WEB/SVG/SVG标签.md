## circle标签

此标签代表圆形，包括三个属性 `cx、cy、r` 分别是中心点的坐标与半径。单位是 `px`

```xml
<svg width="100%" height="100%" viewBox="50 50 50 50">
	<circle id="mycircle" cx="50" cy="50" r="50" />
</svg>
```

上述表示圆心的位置，svg左上角（原点），以及半径 $50$ 

## line标签

用于绘制直线，需要设置两个点坐标

```xml
<svg width="100px" height="100px">

	<line x1="50" y1="50" x2="60" y2="100" stroke="#00BFFF" stroke-width="5px" ></line>

</svg>
```

line默认是没有颜色的，所以必须要指定`stroke`负责是无法看见的

## polyline标签

用于绘制折线，多个点之间用空格隔开，点x、y坐标用 `,` 隔开（可以设置 `fill` 填充色）

```xml
<svg width="100px" height="100px">
<polyline points="10,0 0,10 10,100" stroke="#00BFFF" stroke-width="3px"></polyline>
</svg>
```

## rect标签

用于绘制矩形，指定左上角坐标以及宽高即可

```xml
<svg width="100px" height="100px">
	<rect x="10" y="10" height="20" width="50" />
</svg>
```

默认黑色填充

## ellipse标签

用于绘制椭圆，指定中心点，以及x方向半径y方向半径

```xml
<svg width="100px" height="100px">
	<ellipse cx="10" cy="10" rx="10" ry="50" />
</svg>
```

默认黑色填充

## polygon标签

用于创建多边形，与polyline相同，只是会形成闭合图形（首尾闭合）

```xml
<svg width="100px" height="100px">
	<polygon points="0,0 20,20 0,50" />
</svg>
```

## path标签

绘制路径，使用属性 $d$ 来表示绘制的顺序

- M x,y：移动到
- L x,y：画直线
- Z ：闭合路径
- H x：水平线
- V y：竖直线
- A ：绘制椭圆弧
- Q cx cy x y：贝塞尔曲线
- T x y：跟在 Q 后面，T会从Q终点向x，y生成贝塞尔曲线。控制点未Q控制点关于终点的对称点（T更加平滑）
- C cx1 cy1 cx2 cy2 x y：当前点到xy 的三次贝塞尔曲线
- S sx2 sy2 x y：跟在C后，作用同T（重点控制点为sx2 sy2）

```xml
<svg width="300" height="180">
	<path d="
		M 18,3
		L 46,3
		L 46,40
		L 61,40
		L 32,68
		L 3,40
		L 18,40
		Z
	"></path>
</svg>
```

## text标签

绘制文本

```xml
<svg width="300" height="180">

	<text x="50" y="25">SVG</text>

</svg>
```

## use标签

用于复制一个形状（其他标签）

```xml

```