## webpack

#### 前端工程化

前端工程化：将前端所需额工具、技术、流程、经验等进行规范化、标准化

* 模块化（JS模块化、CSS模块化、资源模块化）
* 组件化（复用休闲游的UI结构、样式、行为）
* 规范化（目录结构规范化、编码规范化、接口规范化、文档规范化、Git分支规范化）
* 自动化（自动化构建、自动部署自动化测试）

目前主流的前端工程化解决方案
* [webpack](https://www.webpackjs.com/)
* [parcel](https://zh.parceljs.org/)

#### webpack 的基本使用

webpack 是前端工程化的解决方案，提供了有好的**前端模块开发支持**，以及**代码压缩混淆**、处理**浏览器端JS的兼容性**、**性能优化**等

---
**基本的测试项目**

首先在项目根目录下初始化：在终端中输入`npm init -y`，就会在根目录下有一个记录的JSON文件`package.json`，这个文件记录了项目的一些信息，包含导入的包

在根目录下创建src目录作为项目源文件的目录，并在根目录下`npm install jquery -S`来添加jquery到dependencies（项目开发到上线都用到的包）中去

最后在src中编写代码，测试如下的案例代码

index.html
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="./index.js"></script>
</head>
<body>
    <ul id="list">

    </ul>
</body>
</html>
```
index.js
```js
// 导入 jquery
import $ from 'jquery'

$(function () {
    for (let i = 0; i < 9; i++) {
        let li = '<li>第' + (i + 1) +  '个 li</li>';
        $('#list').append(li);
    }
    // 奇数
    $('li:odd').css('background-color', 'pink');
    // 偶数
    $('li:even').css('background-color', 'blue');
});
```

但是这样会在浏览器中报错误`Uncaught SyntaxError: Cannot use import statement outside a module`这是由于import是ES6导入语法不兼容的问题

解决方案：导入webpack`npm install webpack@5.42.1 webpack-cli@4.7.2 -D` D与S差不多，D是放到devDependencies下代表只在开发时使用的

配置webpage

在根目录下创建文件`webpack.config.js`并添加如下
```js
// 向外导出webpack的配置对象
module.exports = {
    mode: 'development' 
    // mode 用来指定构建模式 
    // development：开发阶段（不会压缩，时间段，空间大），production：上线（压缩混淆，时间长，空间小）
}
```

在package.json下的script中添加如下，
```JSON
{
"scripts": {
    // 注意：JSON中不能写注释
    // 添加dev脚本，可通过 npm run 名字（dev） 执行
    "dev": "webpack"  
    }
}
```

最后在终端中 `npm run dev`启动 webpack 进行项目的打包构建(有问题，可以使用管理员执行，解决权限问题)

成功后会在**根目录**下创建一个dist目录，下有`main.js`资源文件

将源代码带入的js换为导入main.js即可（main.js将jquery.js和index.js整合到一起）

此时就会发现问题圆满解决，功能实现

>在运行dev脚本，会在运行webpack之前查看配置文件mode的构建模式，决定是否压缩等

* webpack 4.x和5.x版本中，会有如下的默认约定  
    * 默认的打包入口文件为src -> index.js   (没有则报错)
    * 默认的输出文件路径为dist -> main.js   (没有则创建)

可以在`webpack.config.js`配置文件中修改

```js
const path = require('path')

module.exports = {
    // 指定处理的文件
    entry: path.join(__dirname, './src/index.js'),
    // 指定生成的文件
    output: {
        // 存放目录
        path: path.join(__dirname, './dist'),
        // 文件名
        filename: 'index.js' 
    }
}
```


#### webpack插件

* webpack-dev-server
    * 类似nodejs的nodemon工具
    * 当修改源代码会自动打包构建（不会在之前指定的位置，而是存放在内存中，不会直接在磁盘创建，提高速度）默认在项目根目录生成，但是是虚拟的，不可见的，但是可访问
    * 将dev脚本改为`webpack serve`
    * 要查看效果，需要按照命令中提示的默认url访问，而且要更改文件的导入路径为虚拟的文件

* html-webpack-plugin
    * webpack中的HTML插件（类似模板引擎插件）
    * 可以自定义index.html页面的内容

```JS
// webpack.config.js
const HtmlPlugin = require('html-webpack-plugin');

// 插件实例对象
const htmlPlugin = new HtmlPlugin({
    template: './src/index.html',   // 模板文件（原文件）
    filename: './index.html'  // 新文件路径
});

module.exports = {
    mode: 'development',
    plugins: [htmlPlugin],     // 通过 plugins 节点添加插件
}
```

**devServer节点**

在`webpack.config.js`配置文件中，可以通过devServer节点对webpack-dev-server插件进行更多的配置

示例：
```js
devServer: {
	open: true,   // 初次打包，自动打开浏览器
	host: '127.0.0.1',  // 访问的ip地址
	port: 8080,   // 端口
}
```

#### loader

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220615125023.png)

webpack默认只能打包处理.js后缀文件，其他的文件就需要调用loader加载器才可以正常打包

loader加载器的作用：协助webpack打包处理特定的文件模块，如：css-loader、less-loader、babel-loader等

在webpack中一切皆模块，当在index.js中导入index.css模块时，需要loader加载器才能正常打包

示例（打包处理css文件）：

```js
// index.js
// 导入 jquery
import $ from 'jquery'
// 导入css
import './css/index.css'

$(function () {
    for (let i = 0; i < 9; i++) {
        let li = '<li>第' + (i + 1) +  '个 li</li>';
        $('#list').append(li);
    }
    // 奇数
    $('li:odd').css('background-color', 'pink');
    // 偶数
    $('li:even').css('background-color', 'red');
});
```

```css
/* index.css */

#list {
    list-style: none;
    padding: 0;
    margin: 0;
}
```

```js
// webpack.config.json

module.exports = {
    module: {
        // 不同模块对应的规则
        rules: [{ 
            // 匹配的规则
            test: /\.css$/,
            // 使用的方法  从后往前，发现没有了就将更改的数据返回回去
            use: ['style-loader', 'css-loader'] 
        }]
    }
}
```

>在使用打包less时需要安装less和less-loader，在loader规则声明时不用写less，less为内置的

**打包url路径**

如果在样式或者js中有路径相关的则需要下载`url-loader`和`file-loader`同时`file-loader`也是内置的，不用声明，与less相同

当指定的url资源过多会影响资源的加载效率，此时可以在使用规则时用查询字符串的方式添加规则`?limit=INT`当图片资源不大于INT（单位byte）会转为base64格式

两种方法效果相同
```js
// webpack.config.js
{ test: /\.(jpg|png|gif)$/, use: ['url-loader?limit=2000'] },

{ test: /\.(jpg|png|gif)$/,
	use: {
		loader: 'url-loader',  // 指定loader
		options: {    // 指定属性
			limit: '2000'
		}
	}}
```

**打包高级js**

webpack只能打包处理一部分高级的js语法。对于无法处理的高级js语法可以借助于`babel-loader`进行打包处理

```sh
npm i babel-loader @babel/core @babel/plugin-proposal-class-properties -D
```

示例：

```js
// index.js
class Person {
	// 通过 static 关键字，为 Person 类定义了一个静态属性 info
	// webpack 无法打包 静态属性
	static info = 'person info'
}
```

```js
// webpack.config.js
{
	test: /\.js$/,
	// exclude 表示排除项，不处理 node_modules 中的 js 文件
	exclude: /node_modules/,
	use: {
		loader: 'babel-loader',
		options: {
			// 此插件用于转换 class 中的高级语法
			plugins: ['@babel/plugin-proposal-class-properties']
		},
	}
}
```


#### 打包发布

**原因**

项目完成之后，使用webpack对项目进行打包发布
* 开发环境下，打包生成的文件在内存中，无法获取最终打包生成的文件
* 开发环境下，打包生成的文件不会进程代码压缩和性能优化

**配置**

在package.json的script添加脚本

```json
"script": {
	"build": "webpack --mode production"    // 这里的mode会覆盖webpack配置文件中的mode
}
```

打包完成会在根目录下生成dist目录，储存所有的文件

如果想在dist中将不同类型的文件分类，可以在`webpack.config.js`中的output节点配置输出的路径（包含文件名）

```js
// js分类
output: {
	path: path.join(__dirname, './dist'),
	filename: 'js/index.js'
}
```

修改`url-loader`配置项，新增outputPath为对应的文件夹，实现对图片的分类

```js
use: {
	loader: 'url-loader',
	options: {
		limit: '2000',
		outputPath: 'images/',
	}
}},
```

自动清理dist的旧文件

安装`clean-webpack-plugin`插件

在`webpack-config.js`配置自动清除

```js
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const cleanPlugin = new CleanWebpackPlugin();
plugins: [
        htmlPlugin,
        cleanPlugin,
    ],
```

**企业级项目打包发布流程**

* 生成打包报告，根据报告分析具体的优化方案
* Tree-Shaking
* 为第三方库启用 CDN 加载
* 配置组件的按需加载
* 开启路由懒加载
* 自定制首页内容


#### Source Map

在对需要的js源代码进行压缩混淆，减少文件的大小提高文件加载效率时。不可避免的导致了对压缩混淆之后的代码debug非常困难

**Source Map**

Source Map 就是一个信息文件，里面储存这位置信息，就是代码压缩混淆前后的对应关系，在发生错误时，除错工具会根据Source Map 直接显示原始代码，方便后期调试

在开发环境下，webpack默认启用 Source Map 功能。可以直接在控制台提示错误信息的位置，并定位到具体源码

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220616002105.png)

默认Source Map显示的转换后的代码，会导致行号和源文件不同，需要在配置文件中添加`devtool: 'eval-source-map'`属性

在生产环境中，如果省略 devtool 选项，则生成文件不包含 Source Map 能够防止源代码通过 Source Map 的方式泄露（会直接定位到压缩混淆的代码）

可以只定位行数，但不定位源代码，需要将`devtool`值设置为`nosources-source-map`

```js
// webpack.config.js
const path = require('path');
const HtmlPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

const htmlPlugin = new HtmlPlugin({
    template: './src/index.html',   // 模板文件
    filename: './index.html'  // 新文件名（内存中）
});

const cleanPlugin = new CleanWebpackPlugin();

// 向外导出webpack的配置对象
module.exports = {
    mode: 'development',
    // mode 用来指定构建模式
    // development：开发阶段（不会压缩，时间短，空间大），production：上线（压缩混淆，时间长，空间小）
    devtool: 'eval-source-map',  // 保证运行报错行数与源代码行数一致
    entry: path.join(__dirname, './src/index.js'),
    output: {
        path: path.join(__dirname, './dist'),
        filename: 'js/index.js'
    },
    // 通过 plugins 节点添加插件
    plugins: [
        htmlPlugin,
        cleanPlugin,
    ],
    // 指定模块如何被加载
    devServer: {
        port: 3000,
        host: 'localhost',
        open: true,  // 自动打开浏览器
    },
    // loader 配置
    module: {
        rules: [
            { test: /\.css$/, use: ['style-loader', 'css-loader'] },
            { test: /\.less$/, use: ['less-loader'] },
            {
                test: /\.(jpg|png|gif)$/,
                use: {
                    loader: 'url-loader',
                    options: {
                        limit: '2000',
                        outputPath: 'images/',
                    }
                }
            },
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        plugins: ['@babel/plugin-proposal-class-properties']
                    },
                }
            }
        ]
    },
}
```


---
