# JavaScript

## JavaScript 基础
---

###  JavaScript 的认识

* **简介**
    * JavaScript是一种运行在客户端的脚本语言
    * 脚本语言：不需要编译，运行过程由js解析器（js引擎）逐行进行解释并执行
    * 现在也可以基于Node.js技术进行服务器端编程

* **作用**
    * 表单动态检验（密码强度检测）
    * 网页特效
    * 服务端开发（Node.js）
    * 桌面程序（Election）
    * APP（Cordova）
    * 控制硬件-互联网（Ruff）
    * 游戏开发（cocos2d.js）

* **HTML、CSS、JavaScript关系**
    * HTML决定网页的结构和内容
    * JavaScript实现业务逻辑和页面控制
    >HTML、CSS是描述类语言，JS是编程类语言

* **浏览器执行JS**
    * 浏览器分为两个部分：渲染引擎、JS引擎
        * 渲染引擎：用于解析HTML、CSS，俗称内核.如chrome浏览器的blink、老版本的webkit
        * JS引擎：也称为JS解释器，用来读取网页中的JavaScript代码，对齐处理后运行.如：chrome浏览器的V8
    >浏览器本身是不会执行JS代码，而是通过内置的JavaScript引擎来执行JS代码，JS引擎执行代码时逐行解释每一句代码（转换为机器语言），然后交由计算机执行，所有JavaScript语言归为脚本语言，会逐行解析代码

* **JS组成**
    * ECMAScript（JS语法）
        * ECMAScript是由ECMA国际进行的标准化的一门编程语言，JavaScript和JScript是ECMAScript的实现和扩展
        * ***ECMAScript规定了JS的编程语法和核心知识***
    * DOM（页面文档对象模型）
        * DOM是由3C组织推荐的处理可扩展标记语言的标准编程接口，可以对页面上的元素进行操作
    * BOM（浏览器对象模型）
        * BOM提供了独立于内容的、可以与浏览器窗口进行互动的对象结构，通过BOM可以操作浏览器窗口.如：弹窗、控制浏览器跳转、获取分辨率等

* **JS书写位置**
    * 内嵌式
        * 包裹在`<script>JS语句</script>`中一般在`<title>`标签下，同CSS
    
    * 行内式
        * 直接写在元素的属性内部（以on开头的属性）

    * 外联式
        * 写在js文件中，通过`<script scr="JS文件"></script>`引入，引入时，标签中不能写任何代码  
    
    >在script标签中，有一个type属性，默认值为text/javascript代表这个包裹的代码当做js解析

---

### JS基本语法

* **注释**
    * //：单行注释
    * /* */：多行注释

* **输入输出**

|       方法       |              说明              |  归属  |
| :--------------: | :----------------------------: | :----: |
|    alert(msg)    |        浏览器弹出警示窗        | 浏览器 |
| console.log(msg) |    浏览器控制台打印输出内容    | 浏览器 |
|   prompt(info)   | 浏览器弹出输入框，用户可以输入 | 浏览器 |
    >输入的数据是字符型

* **变量**
    * 变量是程序在内存中申请的一块用来存放数据的空间
    * 变量命名：var 变量名;（在程序运行过程中，数据类型会被自动确定）
    * 声明新变量时，也可以使用关键词 "new" 来声明其类型：`var 变量名 = new 变量类型`

* **数据类型**
    * JS中数据的类型是可以变化的
    * 数据类型：
          * 简单数据类型：Number、String、Boolean、Undefined(声明变量但未赋值)、Null
          * 复杂数据类型：object
        * 数字型
            * 进制：
                * 八进制：0 ~ 7 在数字前面加 0 
                * 十六进制：0 ~ 9 a ~ f 在数字前面加 0x
                * 二进制：
            * 数字型范围
                * 最大：Number.MAX_VALUE
                * 最小：Number.MIN_VALUE
            * 数字型三个特殊值
                * 无穷：Infinity（大）、-Infinity（小）
                * NaN：非数字
            >可以用isNaN判断是否是非数字，返回布尔值

        * 字符串
            * 变量名.length：获取字符串长度
            * 字符串 + 任意类型 = 拼接后的新字符串
            * typeof 变量名：获取变量数据类型
    
    * 数据类型转换
        * 转为字符串
        
			|         方式         |             示例              |
			| :------------------: | :---------------------------: |
			|       toString       | `var num = 1; num.toString()` |
			|  String（强制转换）  |  `var num = 1; String(num)`   |
			| ***加号拼接字符串*** | `var num = 1; num = num + ""` |
            
        * ***转为数字型***
        
            | 方式                 | 说明                         | 案例                |
            | :------------------- | :--------------------------- | :------------------ |
            | parseInt(String)     | 将String转换为整数数值型     | `parseInt('78')`    |
            | parseFloat(String)   | 将String转换为浮点数数值型   | `parseFloat('7.8')` |
            | Number()强制转换函数 | 将String转换为数值型         | `Number('12')`      |
            | js隐式转换（. * /）  | 利用算术运算隐式转换为数值型 | '12'.0              |

        * 转为布尔
        
            | 方式      | 示例            |
            | :-------- | :-------------- |
            | Boolean() | Boolean("true") |

* **运算符**
    * 算术运算符：+、-、*、/、%
    * 递增递减运算符：
        * ++i：先自增后返回
        * i++：先返回后自增
    * 比较运算符：<、>、\==、>=、<=、\=\==、!=、!==
        * \=\==、!\==：全等，要求 ***值和数据*** 都相同
    * 逻辑运算符：&&、||、!
        * 短路运算：当多个表达式时前面的表达式已经可以决定结果时，不用继续计算
            * 逻辑与
            * 表达式1 && 表达式2
            * 表达式1为true返回表达式2，否则返回表达式1
            ---
            * 逻辑或
            * 表达式1 || 表达式2
            * 表达式1为true返回表达式1，否则返回表达式2

    * 赋值运算符
        * 变量 = 表达式;、变量 += 表达式; 等
        * 将表达式的值赋给变量
        * ***如果将值赋给未定义的变量时，将会被自动作为widows的一个属性***
        * 没有声明的变量可配置全局属性，可以删除

* **流程控制**
    * **if-else**
        ```js
        if (判断条件) {
            执行语句
        }
        else if (判断条件) {
            执行语句
        }
        else {
            执行语句
        }
        ```
    
    * 三元表达式
        * 条件表达式 ? 表达式1 : 表达式2
        * 如果条件表达式为true则返回表达式1的值，否则返回表达式2的值

    * **switch**
        ```js
        switch (表达式) {
            case value1: 
                执行语句1;
                break;
            case value2: 
                执行语句2;
                break;
            default:
                执行最后语句;
                break;
        }
        ```
    
    * **for —— for/in**
        ```js
        for (语句1; 语句2; 语句3) {
            执行语句;
        }
        ```
        * 语句1：循环开始前执行
        * 语句2：循环条件
        * 语句3：每次循环后执行
        ```js
        for (属性名 in 可遍历对象) {
            执行语句;
        }
        ``` 
    
    * **while —— do-while**
        ```js
        while (条件语句) {
            执行语句;
        }
        ```
        * 先判断条件后执行语句
        ```js
        do {
            执行语句;
        }
        while (条件语句);
        ```
        * 先执行语句后判断条件，至少执行一次

    * **break —— continue**
        * break会跳出整个循环，执行循环下面的代码
        * continue会跳出此次循环，执行下一次循环

* **对象**
    * 自定义对象
        * var 对象名 = new object();
        * 对象是键值对的容器{name: value}，键值对在JS中通常称为对象属性
        * 添加属性：对象名.属性名 = 属性值
        * 添加方法：对象名.方法名 = function() {代码块}
	```js
	var person = {name: 'Ail',
				age: 28,
				ph: '123456789',
				sex: '男',
				函数名: function() {代码块}
				}
	```
    * 内置对象
        * JS自带的对象，可以直接使用实现一些效果
            * 例如：数组对象
            * `var 数组名 = new Array();`
            
			| 方法名                                 | 说明                                               |
			| :------------------------------------- | :------------------------------------------------- |
			| push()                                 | 在数组末尾添加数据，并返回新长度                   |
			| pop()                                  | 删除末尾元素，并返回被删除的元素                   |
			| unshift()                              | 在数组前面添加数据，并返回新长度                   |
			| shift()                                | 删除第一个元素，并返回被删除的元素                 |
			| splice(索引,要删除元素个数,添加的数据) | 在数组指定位置添加数据                             |
			| sort(function(a, b){return 值})        | 值 = a - b（升序）值 = b - a（降序），返回新数组   |
			| reverse()                              | 翻转数组，返回新数组                               |
			| indexOf()                              | 从前往后查找返回给定元素第一个索引，如果没有返回-1 |
			| LastIndexOf()                          | 从后往前，同indexOf                                |
			| toString()                             | 将数组转换为字符串逗号隔开，返回字符串             |
			| join(分隔)                             | 同toString默认逗号，可填分隔                       |

    * 浏览器对象
        <!-- TODO -->
                   

* **函数**
    * 函数使用
        ```js
        // 声明函数方法1
        function 函数名(参数) {
            代码块;

            return 数值; // 函数返回值(可选)
        }

        // 声明函数方法2
        var 变量名 = function() {代码块}

        // JS中调用函数
        函数名(参数);

        // 行内式调用函数
        <button onclick="函数名(参数)"></button>  
        ```
    * 构造函数
        * 构造函数首字母大小，不需要return就可以返回一个对象
        * 将对象中相同的 ***属性和方法*** 封装到函数中
        ```js
        // 构造函数
        function 构造函数名() {
            this.属性名 = 属性值;
            this.方法名 = function() {代码块}
        }
        // 调用构造函数
        new 构造函数名();
        ```
        示例:
        ```js
        function Start(uname, age, sex) {
            this.uname = uname;
            this.age = age;
            this.sex = sex;
            this.sing = function(sang) {
                console.log(sang);
            }
        }
        var ldh = new Start('刘德华', 18, '男');
        ldh.sing('唱歌!');
        console.log(ldh.uname);
        ``` 
        >new关键字执行过程：先在内存中创建一个空对象\
        >this就会指向这个空对象\
        >执行代码构造函数代码然后给这个空对象添加属性、方法\
        >最后返回这个对象

        >在不确定有多少实参传入时，arguments存储了所有的实参，值按传入顺序 (数组)


> ***变量、属性、函数、方法的区别***\
>变量和属性都是用来存储数据的，变量单独声明，使用时直接写变量名.属性在对象中，不需要声明，使用时必须是对象名.属性名\
>函数和方法都是实现某种功能，函数单独定义，调用时 函数名(参数);.方法在对象中，调用的时候 对象名.方法名(参数)

* **作用域**
    * 可访问的变量、函数、对象的集合
    * 变量在函数内声明，变量为局部作用域只能在函数内部访问，因为是局部变量，所有不同的函数可以有相同的变量名，局部变量在函数执行结束后就会被销毁
    * 变量在函数外声明或在函数内没有声明（没有var关键字），变量为全局作用域，全局变量只能有一个相同名字，在网页关闭的时候销毁
---
---

## BOM和DOM

[JS对象查阅手册](https://www.runoob.com/jsref/jsref-tutorial.html)

---

## API和WebAPI
* API
    * API是一些预先定义的函数工具，以便更轻松的实现想要完成的功能
* WebAPI
    * 是浏览器提供的一些操作浏览器功能和页面元素的API（BOM、DOM）
---

## DOM

[document对象手册](https://www.runoob.com/jsref/dom-obj-document.html)

* DOM（文档对象模型），是处理HTML或者XML的程序接口
* DOM树
![DOM树](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimage.morecoder.com%2Farticle%2F201810%2F20181024121100924078.gif&refer=http%3A%2F%2Fimage.morecoder.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1645775144&t=045d90ff45b71447a5e81c606f7b24f5)
    * 文档：一个页面就是一个文档，DOM中用document表示
    * 元素：页面中的所有标签都是元素，DOM中用element表示
    * 节点：网页的所有内容都是节点，DOM中用node表示
    >DOM将以上内容都看作对象

---
### ***获取页面元素***

DOM在实际开发中主要用于操作元素

* **根据ID获取**
    * 使用getElementById('id')方法获取元素，返回一个元素对象
        ```html
        <body>
            <div id="time">2022-1-26</div>
            <script>
                var timer = document.getElementById('time');
                // 打印返回元素对象
                console.dir(timer);
            </script>
        </body>
        ```
* **根据标签名获取**
    * 使用getElementsByTagName('标签名')方法获取带有标签名的对象集合，以伪数组的形式返回
        ```html   
        <body>
            <ul>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
            </ul>
            <script>
                var lis = document.getElementsByTagName('li');
                // 指定: 父元素.getElementsByTagName(子元素);
                // 指定的父元素必须是单个对象
            </script>
        </body>
        ```
* **通过HTML5新增的方法获取（不支持i9以下）**
    * 使用getElementsByClass('类名')方法获取
        ```html
        <body>
            <div class="time">2022-1-26</div>
            <script>
                var timer = document.getElementByClass('time');
            </script>
        </body>
        ```
    * querySelector('选择器')方法返回指定选择器的元素集合第一个元素

    * querySelectorAll('选择器')方法返回指定选择器的所有元素

---
### **事件**

* 事件组成：（事件三要素）
    * 事件源：事件被触发的对象
    * 事件类型：事件被触发的类型[事件类型参考](https://www.runoob.com/jsref/dom-obj-event.html)
    * 事件处理程序：通过一个函数赋值的方式完成

    示例:    
    ```html
    <body>
        <button id="btn">按钮</button>
        <script>
            var btn = document.getElementById('btn');
            // onclick：鼠标点击
            btn.onclick = function() {alert('按钮！');}
        </script>
    </body>
    ```

* 执行事件的步骤
    * 获取事件源
    * 注册事件（绑定事件）
    * 添加事件处理程序（通过函数赋值方式）

--
### **操作元素**

* 改变元素内容
    * innerText、innerHTML
    
    示例:
    ```html
    <body>
        <button>显示当前时间</button>
        <div>某个时间是:</div>
        <script>
            var btn = document.querySelector('button');
            var div = document.querySelector('div');
            btn.onclick = function() {
                var date = new Date();
                var year = date.getFullYear();
                var month = date.getMonth() + 1;
                var dates = date.getDate();
                var arr = [ '星期日', 
                            '星期一',
                            '星期二',
                            '星期三',
                            '星期四',
                            '星期五',
                            '星期六'
                        ]
                var day = date.getDay();
                div.innerHTML = year + '-' + month + '-' + dates + '-' + arr[day];
            }
        </script>
    </body>
    ```

    >innerHTML识别html语句，innerText不识别html语句而且会去除换行（会直接输出）
    >
    >都是可读写的，可以获取元素里面的内容

* **修改元素属性**

    示例：(点击按钮更改图片)
    ```html
    <body>
        <button id="zxy"></button>
        <button id="ldh"></button>
        <img src="#" alt="" title="">
        <script>
            var zxy = document.getElementById('zxy');
            var ldh = document.getElementById('ldh');
            zxy.onclick = function() {
                img.src = 'http://';
                img.title = '张学友';
            }
            ldh.onclick = function() {
                img.src = 'http://';
                img.title = '刘德华';
            }
        </script>
    </body>    
    ```
    
* **表单元素属性的操作**
* 
    示例:（登录密码显示和隐藏）
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>登录界面</title>
        <style>
            * {
                margin: 0;
                padding: 0;
            }

            .box {
                width: 300px;
                height: 600px;
                border-radius: 50px;
                border-color: #111;
                margin: 100px auto;
            }
            
            input {
                
                color: #808080;
                width: 200px;
                margin: 0 auto;
                outline: none;
                border: 0;
                border-bottom: 1px solid #ccc;
            }

            button {
                float: right;
                border: 0;
            }

        </style>
    </head>
    <body>
        <div class="box">
            <div>
                <input type="text" placeholder="请输入账号">
            </div>
            <div>
                <input type="password" placeholder="请输入密码" id="pwd">
                <button id="eye">显示密码</button>
            </div>
        </div>
        <script>
            var input_password = document.getElementById("pwd");
            var btn = document.getElementById("eye");
            var flag = 0;
            btn.onclick = function() {
                if (flag == 0) {
                    input_password.type = 'text';  
                    flag = 1;  
                }
                else {
                    input_password.type = 'password';
                    flag = 0;
                }   
            }
        </script>
    </body>
    </html>
    ```

* **修改样式属性**
    ```js
    样式比较少时使用
    属性名是采用的驼峰命名法，不同于CSS的background-color
    element.style.backgroundColor: "red";
    
    样式较多时使用类来更改(会覆盖之前的类)
    element.className = "one";
    ```

* **排他思想**
    * 如果同一组元素，想要实现某一个元素相同的样式：
        * 先将所有元素样式清空
        * 再给当前元素添加样式
        
    示例:(按钮切换)
    ```html
    <body>
        <button>按钮1</button>
        <button>按钮2</button>
        <button>按钮3</button>
        <button>按钮4</button>
        <button>按钮5</button>
        <script>
            var btns = document.getElementsByTagName('button');

            for (var i = 0; i < btns.length; i++) {
                btns[i].onclick = function() {
                    // 先清除所有的背景，以达到点击按钮时，其他按钮全部没有背景
                    for (var j = 0; j < btns.length; j++) {
                        btns[j].style.backgroundColor = '';
                    }
                    // 让当前按钮改为pink
                    this.style.backgroundColor = 'pink';
                }
            }
        </script>
    </body>
    ```

    示例: （表单全选/取消全选）

    ```html
    <div class="wrap">
        <table>
            <thead>
                <tr>
                    <th>
                        <input type="checkbox" id="j_cbAll">
                    </th>
                    <th>商品</th>
                    <th>价钱</th>
                </tr>
            </thead>
            <tbody id="j_tb">
                <tr>
                    <td>
                        <input type="checkbox">
                    </td>
                    <td>iPhone8</td>
                    <td>8000</td>
                </tr>
                <tr>
                    <td>
                        <input type="checkbox">
                    </td>
                    <td>iPhone8</td>
                    <td>8000</td>
                </tr>
                <tr>
                    <td>
                        <input type="checkbox">
                    </td>
                    <td>iPhone8</td>
                    <td>8000</td>
                </tr>
                <tr>
                    <td>
                        <input type="checkbox">
                    </td>
                    <td>iPhone8</td>
                    <td>8000</td>
                </tr>
                <tr>
                    <td>
                        <input type="checkbox">
                    </td>
                    <td>iPhone8</td>
                    <td>8000</td>
                </tr>
            </tbody>
        </table>
    </div>
    <script>
        // 全选按钮
        var j_cbAll = document.getElementById('j_cbAll');
        // 单个按钮
        var j_tb = document.getElementById('j_tb').getElementsByTagName('input');
        j_cbAll.onclick = function() {
            for (var i = 0; i < j_tb.length; i++) {
                j_tb[i].checked = this.checked;
            }
        }
         // 如果下面的按钮全部选择时，全选按钮自动勾上
        for (var i = 0; i < j_tb.length; i++) {
            j_tb[i].onclick = function() {
                var flag = true;
                // 每次点击，检查所有的是否都是选择
                for (var j = 0; j < j_tb.length; j++) {
                    if (!j_tb[j].checked) {
                        flag = false;
                        break;
                    }
                }
                j_cbAll.checked = flag;
            }
        }
    </script>
    ```

* **自定义属性的操作**
    * 目的：为了保存并使用数据，有些数据可以保存再页面中而不用保存在数据库中，规定以data开头
    
    * 获取属性值
        * `element.属性`获取内置属性值
        * `element.getAttribute('属性')`主要获取自定义属性值
 
    * 设置属性值
        * `element.属性 = '值'`设置内置属性值
        * `element.setAttribute('属性', '值')`主要设置自定义属性值

    * 移除属性
        * `element.removeAttribute('属性')`

    示例（tab切换栏）
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>页面</title>
        <style>
            * {
                margin: 0;
                padding: 0;
                box-sizing: border-box;    
            }

            .table {
                width: 800px;
                background-color: #fff;
                margin: 100px auto;
            }

            .current {
                background-color: red;
                color: #fff;
            }

            .tab_list {
                border: 1px solid gray;
                height: 39px;
                background-color: rgb(238, 222, 222);
                margin-top: 10px;
            }

            .tab_con {
                margin-top: 10px;
                height: 100px;
            }

            ul {
                list-style: none;
            }

            .tab_list li {
                float: left;
                line-height: 39px;
                padding: 0 20px;
                text-align: center;
                cursor: pointer;
            }

            .item_info {
                padding: 20px 0 0 20px;
            }

            .item {
                display: none;
            }

        </style>
    </head>
    <body>
        <div class="table">
            <div class="tab_list">
                <ul>
                    <li class="current">商品介绍</li>
                    <li>规格与包装</li>
                    <li>售后保障</li>
                    <li>商品评价</li>
                    <li>手机社区</li>
                </ul>
            </div>
            <div class="tab_con">
                <div class="item" style="display: block;">
                    商品介绍内容
                </div>
                <div class="item">
                    规格与包装内容
                </div>
                <div class="item">
                    售后保障内容
                </div>
                <div class="item">
                    商品评价内容
                </div>
                <div class="item">
                    手机社区内容
                </div>
            </div>
        </div>

        <script>
            var lis = document.querySelector('.tab_list').querySelectorAll('li');

            var items = document.querySelectorAll('.item');

            for (var i = 0; i < lis.length; i++) {
                lis[i].setAttribute('date-index', i);

                lis[i].onclick = function() {
                    for (var j = 0; j < lis.length; j++) {
                        lis[j].className = '';
                    }
                    this.className = 'current';
                    var index = this.getAttribute('date-index');

                    for (var j = 0; j < items.length; j++) {
                        items[j].style.display = 'none';
                    }
                    items[index].style.display = 'block';
                }          
            }
        </script>
    </body>
    </html>
    ```

---
### **节点操作**

* **获取元素的两种方式**
    * 利用DOM的方法获取元素，逻辑性不强、繁琐（元素操作）
    * 利用节点层级关系获取元素 逻辑性强、兼容性差 （节点操作）

    >相比较之下，节点操作更为简单

* **节点描述**
    * 网页中的所有内容（包括空格换行）都是节点，用node表示.HTML DOM树中的所有节点均可通过JS进行访问，都可以被创建或删除

    * 一般的节点都有nodeType、nodeName、nodeValue三个基本属性

        * 元素节点：nodeType 为 1
        * 属性节点：nodeType 为 2
        * 文本节点：nodeType 为 3（包括空格、换行、文字）
        
* **节点层级**
    * 利用DOM树可以将节点划分为不同的层级关系
    
        * 父节点

            ```js
            // 获取子节点
            var son = document.getElementById('son');
            // 离son最近的父节点，如果没有，返回空
            var parent = son.parentNode;
            ```
        
        * 子节点
            * 标准

            ```js
            var parent = document.getElementById('parent');
            // 所有的子节点，包括空格和换行
            var son = parent.childNodes;
            // 可以利用nodeType来筛选想要的元素
            ```

            * 非标准
 
            ```js
            var parent = document.getElementById('parent');
            // 获取所有子元素节点
            var son = parent.children;
            ```

            * firstChild、lastChild分别时获取第一个和最后一个子节点（所有的子节点中选取）

            * firstElementChild、lastElementChild分别是获取第一个和最后一个子元素节点（子元素节点中选取 IE9一下不兼容）
            
    * 兄弟节点
        * node.nextSibling 获取node的下一个 ***节点***（包括空格、换行）未找到返回Null
        * node.previousSibling 获取node的上一个节点
        * node.nextElementSibling 获取下一个 ***元素节点***
        * node,previousElementSibling 获取上一个 ***元素节点***
        
        封装一个兼容性函数，解决兼容性问题
        ```js
        function getNextElementSibling(element) {
            var el = element;
            while (el = el.nextSibling) {
                if (el.nodeType === 1) {
                    return el;
                }
            }
            return null;
        }
        ```

    * 创建节点、添加节点
        * `document.createElement('元素')` 创建元素节点
        * `node.appendChild(child)` 在末尾添加节点
        * `node.insertBefore(child, 元素)`在前面添加元素
        >创建的是一个孩子元素节点，应用于评论区等

    * 删除节点
        * `node.removeChild(元素)`删除指定子元素节点
    
    * 复制节点
        * `node.cloneNode(布尔值)`克隆指定元素节点，默认为false，为浅拷贝，只复制标签.改为true为深拷贝，复制标签和内容
        
    ***三种创建元素的区别***
    
    * `document.write()`
        * 直接写入页面的内容，但是文档流执行完毕，则会导致页面全部重绘
    * `element.innerHTML`
        * 采用字符串拼接的形式，数据多时，效率低下
        * 但如果采用数组形式，效率比createElement()高
        
            ```js
            采用字符串拼接形式
            var inner = document.querySelector(选择器);
            for (var i = 0; i < 次数; i++) {
                inner.innerHTML += 元素代码
            }

            采用数组的形式添加
            var arr = [];
            for (var i = 0; i < 个数; i++) {
                arr.push(元素代码);
            }
            document.父元素.innerHTML = arr.join('');
            ```
    
    * `document.createElement()`
        * 效率高，好用


* ***DOM重点核心***
    * 关于DOM的操作，主要针对元素的操作，主要有创建、增、删、改、查、属性操作、事件操作

* ***事件高级***

    * **元素注册事件的两种方式**
        * `element.事件 = function() {}`  
            >传统方法on开头的，具有唯一性,同一元素只能设置一个函数(后面的会覆盖前面的)
        * `addEventListener(type, listener[, useCapture])`    
            >利用方法监听事件，同一元素同一事件可以注册多个监听事件
            
            * type: 事件类型字符串如：click（不带on）
            * listener：事件处理函数（不需要调用，只用写函数名）
            * useCapture：可选参数，默认false，与DOM事件流有关

    * **删除事件的两种方式**
        * `eventTarget.事件 = null;`
            >传统的解绑事件方式

        * `eventTarget.removeEventListener(type, listener[, useCapture])`
            >解绑事件监听

    * **DOM事件流**
        * 事件发生时会在元素节点之间按照特定的顺序传播，这个传播过程就是DOM事件流
        ![DOM事件流](http://www.droliz.cn/markdown_img/DOM事件流.jpg)

            DOM事件流的三个阶段：捕获阶段 -> 当前目标阶段 -> 冒泡阶段
        
        * 传统的事件添加是在冒泡阶段调用事件处理程序，如果添加事件监听的useCapture的值是true，表示事件在捕获阶段就调用事件处理程序，否则就是在冒泡阶段调用事件处理程序
                                                                * ***事件对象***
        * 对于不同的事件，事件对象（event简写e）有不同的属性和方法，写在function（事件对象）中，不用传入实参，系统自动创建

            | 属性和方法          | 说明                           |
            | :------------------ | :----------------------------- |
            | e.target            | 返回出发事件的对象             |
            | e.type              | 返回事件类型，不带on           |
            | e.preventDefault()  | 阻止默认事件，如：不让链接跳转 |
            | e.stopPropagation() | 阻止冒泡                       |

            [更多](https://www.runoob.com/jsref/dom-obj-event.html)      

    * **阻止冒泡**
        * 利用事件对象的`stopPropagation()`方法

    *  **事件委托**
        * 事件委托也叫时间代理，在jQuery里面称为事件委派
        * 给父节点设置事件监听，然后利用冒泡原理来邮箱设置的每个子节点.提高性能

---
---

## BOM
---
### BOM基础

[window对象手册](https://www.runoob.com/jsref/obj-window.html)

* BOM是 ***浏览器对象模型***，提供了独立于内容而与浏览器窗口进行交互的对象，其核心对象是window
* BOM缺乏标准，JS语法的标准化组织是ECMA，DOM的标准化组织是W3C，BOM最初是Netscape浏览器标准的一部分

* **BOM构成**
    * window对象是浏览器的顶级对象，它具有双重角色
        * 他是JS访问浏览器窗口的一个接口
        * 他是一个全局变量，定义在全局作用域中的变量、函数、都会变成window的对象方法
        ![BOM构成](http://www.droliz.cn/markdown_img/BOM构成.jpg)
        >window下有一个特殊属性 window.name

* **window对象的常见事件**
    * 窗口加载事件
        ```js
        // 表示整个文件加载完才会触发（包括图像、脚本文件、CSS文件等）
        window.onload = function() {代码块}（只能写一次）

        window.addEventListener("load", function(){代码块})

        document.addEventListener("DOMContentLoaded", function() {代码块})
        ```
        >DOMContentLoaded是DOM加载完毕，不包括图片、CSS、flash等，就可以执行，比load快，load是将页面全部内容加载完毕才执行
    
    * 调整窗口大小事件
        ```js
        // 窗口大小发生变化时，就会调用
        window.onresize = function() {}

        window.addEventListener("resize", function() {代码块})
        ```

* **定时器**

    * window中提供了两种定时器（方法）
        * setTimeout(调用函数, [延迟毫秒数]) 
            * window可以省略
            * 当毫秒数结束时，调用函数（**只执行一次**）
            * 一般来说为了区分，都会给定时器起一个标识符（变量名）
            * ***回调函数***
                * 需要等待时间，时间到了才回去调用这个函数    
            * `clearTimeout(timeoutId)`可以终止定时器
        * setInterval(函数, 时间间隔毫秒)
            * 每隔时间间隔调用一次（**重复调用**）
            * `clearInterval`可以清除定时器
            
        ```html
        <!-- 验证码的间隔发送 -->
        <script>
            var btn = document.querySelector('button');
            var input = document.querySelector('input');
            var placeholder = null;
            input.addEventListener('focus', function() {
                placeholder = this.placeholder;
                this.placeholder = "";
            })
            input.addEventListener('blur', function() {
                this.placeholder = placeholder;
            })

            btn.addEventListener('click', function() {
                this.disabled = true;
                var times = 60; 
                <!-- 标识符 -->
                var change_timer = setInterval(function() {
                    if (times == 0) {
                        btn.disabled = false;
                        btn.innerHTML = '发送';
                        clearInterval(change_timer);

                    }else {
                        btn.innerHTML = '还剩下' + times + 's';
                        times--;
                    }
                }, 1000);
            })
        </script>    
        ```
    ***this指向问题***
        
    在定义函数的时候this的指向是不确定的，只有函数执行的时候才能确定this的指向，一般情况下this的最终指向是调用它的对象

    * 在**全局作用域**或**普通函数**中this指向全局对象window（在定时器指向window）
    * 在方法调用中，this指向掉头用者
    * 构造函数中this指向构造函数的实例

* **JS执行队列**
    * JS是单线程，同一时间只能做一件事.如：对于一个DOM元素进行添加和删除，只能先添加后删除，不能同时进行
        ![JS执行机制](http://www.droliz.cn/markdown_img/JS执行机制.jpg)

    * *同步*
        * 第一个任务结束后，再执行后一个任务，程序的执行顺序与任务的排列顺序是一致的、同步的
        * 同步任务：同步任务都在主线程上执行，形成一个 ***执行栈***

    * *异步*
        * 在处理一个任务的时候，可以处理其他的任务
        * 异步任务：JS的异步是通过 ***回调函数*** 实现的，放进任务队列中（通过异步进程处理判断是否放入队列，如点击事件在点击之后才会加入队列，定时器会在定时结束放入） 

    >先执行执行栈中的同步任务，再将异步任务放入任务队列中，当执行栈中的所有同步任务执行完成，按次序读取任务队列中的异步任务，结束等待状态，进入任务栈中，开始执行

    由于在主线程不断地重复获得任务、执行任务，这种机制称为**事件循环**（event loop）

* **location对象**
    * 用于获取或设置窗体的URL，并且可以用于解析URL

        * URL语法格式：`protocol://host[:port]/path/[?query]#fragment`
            * protocol: 通信协议（http、ftp等）
            * host: 主机（域名）
            * port: 端口号
            * path: 路径
            * query: 参数，以键值对形式通过&分隔
            * fragment: 片段，#后内容常见于链接锚点

        示例
        ```html
        <!-- 获取登录用户的参数 -->
        // 登录界面
        <form action="index.html">
            <input type="text" name="userId">
            <input type="submit" value="登录">
        </form>

        // 跳转界面
        <script>
            // user = [userId, 输入的用户名] 
            var user = location.search.substr(1,).split('=');
            // 先获取键值对，再去掉开头的问号，然后以等号分隔字符串
        </script>
        ```

        | 方法               | 返回值                                                  |
        | :----------------- | :------------------------------------------------------ |
        | location.assign()  | 和href一样，也可以跳转，但记录历史，可以返回之前的页面  |
        | location.replace() | 同assign，但不记录历史                                  |
        | location.reload()  | 重新加载页面相当于F5，如果参数为true则强制刷新ctrl + F5 |

* **navigator对象**
    * 包含浏览器的相关信息，最常用的userAgent可以返回页面头部信息，可以根据头部信息，判断是跳转PC端页面还是PE端页面

* **history对象**
    * 主要是与浏览器历史记录进行交互，该对象包含用户再浏览器窗口中访问过的所有URL

---
### PC端页面特效
            
* **元素偏移量**
    * offset的相关属性可以**动态的**获取元素的位置、大小等
        * 获得元素距离 ***带有定位*** 父元素的位置
        * 获取元素自身的大小
        * 所有获取的参数均不带单位

    >style只能得到行内样式表中的样式值，offset可以得到任意样式值，获取元素的样式值用offset更合适，更改元素样式值用style更合适

    应用：一般的想要得到鼠标再盒子中的位置，先得到鼠标在页面中的位置(e.pageX, e.pageY)再获取盒子再页面中的距离(box.offsetLeft, box.offsetTop)相减就可得到鼠标再盒子中的位置

    应用分析: （拖拽盒子）    
    * 先点击鼠标，再移动鼠标，然后放开鼠标，实现鼠标的移动.（先点击再移动，所以 mousemove 事件和 mouseup 事件放在 mousedown 事件中）
    * 当鼠标落下时，获得鼠标再盒子内的坐标和鼠标再页面的坐标，相减就是盒子的坐标，赋值给盒子，当鼠标松开解除移动事件

    应用分析：（图片局部放大）
    * 局部放大图片先明确鼠标移动的范围，

* **元素可视区**
    * client的相关属性可以获取元素可视区的相关信息，通过client的相关属性可以动态的获取元素的边框大小、元素大小等（相比offset，client宽度不包括边框的大小）

* ***立即执行函数***
    * 不需要调用，立即就能执行的函数，如下两种写法：
        * (function() {}())
        * (function() {})()
    * 主要用于创建一个独立的执行域，避免命名冲突的问题

* **元素滚动**
    * scroll相关属性可以动态获得该元素的大小、滚动距离等

* ***总结***
    * offset系列经常用于获取元素的位置
    * client系列经常用于获取元素的大小
    * scroll系列经常用于获取滚动距离

    >当元素溢出时，可以通过overflow属性选择是否显示溢出内容

* **动画**
    * 动画实现的原理
        * 核心原理：通过定时器setInterval不断移动盒子的位置
        * 1、获得盒子当前位置
        * 2、让盒子当前位置上加上一个移动距离
        * 3、利用定时器不断重复这个操作
        * 4、加一个结束定时的条件
        * 此元素需要添加定位，才能使用element.style.left

        >对于一个封装好的动画函数，每次调用都会在内存中申请空间用于存储计时器，比较浪费资源，但可以将每个获取过来的对象添加一个属性，将定时器添加进去

        ```js
        function(obj, target) {
            obj.name = setInterval(function() {
                // 定时器执行语句块
            }, 30);
        }
        ```

    * 缓动效果原理
        * 缓动动画就是让元素运动速度有所变化，最常见的是让速度慢慢停下来
            * 让盒子每次移动的距离慢慢变小
            * 核心算法：（目标值 - 现在的位置） / 10 作为每次移动的距离步长(根据具体情况更改)

    * 动画函数添加回调函数
        * 回调函数原理：函数可以作为一个参数.将这个函数作为一个参数传到另一个函数里面当那个函数
        执行完之后，再执行传进去的这个函数，这个过程叫做 ***回调***
    
        * 回调函数位置：定时器结束的位置

        ```html
        <div></div>
        <button class="btn500px">500px</button>
        <button class="btn800px">800px</button>
        <script>
            function animate(obj, target, callback) {
                clearInterval(obj.timer);
                obj.timer = setInterval(function () {
                    var step = Math.ceil((target - obj.offsetLeft) / 10)
                    step = step > 0 ? Math.ceil(step) : Math.floor(step);

                    if (obj.offsetLeft == target) {
                        clearInterval(obj.timer);
                        // 回调函数
                        if (callback) {
                            callback();
                        }
                    }
                    obj.style.left = obj.offsetLeft + step + 'px';

                }, 15);
            }

            var div = document.querySelector("div");
            var btn_5 = document.querySelector(".btn500px");
            var btn_8 = document.querySelector(".btn800px");

            btn_5.addEventListener('click', function () {
                animate(div, 500, function () {代码块});
            });
            btn_8.addEventListener('click', function () {
                animate(div, 800, function () {代码块});
            });
        </script>
        ```

    >由于会经常使用动画，可以单独将动画封装到一个JS文件中，需要使用的时候调用即可

* **节流阀**
    * 当上一个函数执行完毕时再去执行下一个函数，避免事件连续触发太快
    * 核心实现思路：利用回调函数，添加一个变量来控制，锁住函数以及解锁函数
    * 再动画中设置变量 flag = true，执行时变为false 此时禁止下次动画，回调函数执行时将flag = true

---

### 本地存储

* **简介**
    * 将数据存储再用户的浏览器中
    * 设置、读取方便，甚至页面刷新也不会丢失数据

* **两种本地存储的方式**
    * sessionStorage属性
        * 生命周期为关闭浏览器窗口
        * 再同一个窗口（页面）下数据是共享的
        * 以键值对方式存储

    * localStorage属性
        * 生命周期永久，除非主动删除
        * 可以多个窗口
        * 以键值对方式存储

    * setItem(key, value) 方法存储数据
    * getItem(key) 方法可以获取数据
    * removeItem(key) 方法可以删除数据
    * clear() 方法可以删除所有数据


## 注意

浏览器总是习惯性的打印出你执行函数后的返回值，所以在有些情况下使用`console.log()` 打印会在末尾打印一个`undefined`

## 其他

### sql注入

#### 防止输入框sql注入

```js
// 防止输入框 sql 注入
preSql = function (obj){
	var dom = $(obj);
	var re = /select|update|delete|exec|count|'|"|=|;|>|<|%/i;
	if (re.test(dom.val().toLowerCase())){
		alert("请勿输入非法字符！");
		dom.val("");
	}
}

inputs = $('input');
// console.log(inputs);

inputs.each(function(index, item){
	$(item).on('input', function(){
		preSql(this);
	});
});
```


### 防抖和节流

防抖：在事件被触发 n 秒后再执行回调，如果在这 n 秒内又被触发，则重新计时
-   给按钮加函数防抖防止表单多次提交。
-   对于输入框连续输入进行AJAX验证时，用函数防抖能有效减少请求次数。
-   判断scroll是否滑到底部，滚动事件+函数防抖

节流：指定时间间隔 n 内只会执行一次任务。
-   游戏中的刷新率
-   DOM元素拖拽
-   Canvas画笔功能

**防抖**
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>防抖</title>
</head>
<body>
  <button id="debounce1">点我防抖！</button>

  <script>
    window.onload = function() {
      // 1、获取这个按钮，并绑定事件
      var myDebounce = document.getElementById("debounce1");
      myDebounce.addEventListener("click", debounce(handle);
    }

    // 2、防抖功能函数，接受传参
    function debounce(fn) {
      // 4、创建一个标记用来存放定时器的返回值
      let timeout = null;
      return function() {
        // 5、每次当用户点击/输入的时候，把前一个定时器清除
        clearTimeout(timeout);
        // 6、然后创建一个新的 setTimeout，
        // 这样就能保证点击按钮后的 interval 间隔内
        // 如果用户还点击了的话，就不会执行 fn 函数
        timeout = setTimeout(() => {
          fn.call(this, arguments);
        }, 1000);
      };
    }

    // 3、需要进行防抖的事件处理
    function handle() {
      // 有些需要防抖的工作，在这里执行
      console.log("防抖成功！");
    }

  </script>
</body>
</html>
```

**节流**
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>节流</title>
</head>
<body>

  <button id="throttle">点我节流！</button>

  <script>
    window.onload = function() {
      // 1、获取按钮，绑定点击事件
      var myThrottle = document.getElementById("throttle");
      myThrottle.addEventListener("click", throttle(sayThrottle));
    }

    // 2、节流函数体
    function throttle(fn) {
      // 4、通过闭包保存一个标记
      let canRun = true;
      return function() {
        // 5、在函数开头判断标志是否为 true，不为 true 则中断函数
        if(!canRun) {
          return;
        }
        // 6、将 canRun 设置为 false，防止执行之前再被执行
        canRun = false;
        // 7、定时器
        setTimeout( () => {
          fn.call(this, arguments);
          // 8、执行完事件（比如调用完接口）之后，重新将这个标志设置为 true
          canRun = true;
        }, 1000);
      };
    }

    // 3、需要节流的事件
    function sayThrottle() {
      console.log("节流成功！");
    }

  </script>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>节流</title>
</head>
<body>

  <button id="throttle">点我节流！</button>

  <script>
    window.onload = function() {
      // 1、获取按钮，绑定点击事件
      var myThrottle = document.getElementById("throttle");
      myThrottle.addEventListener("click", throttle(sayThrottle));
    }

    // 2、节流函数体
    function throttle(fn) {
      // 4、通过闭包保存一个标记
      let canRun = true;
      return function() {
        // 5、在函数开头判断标志是否为 true，不为 true 则中断函数
        if(!canRun) {
          return;
        }
        // 6、将 canRun 设置为 false，防止执行之前再被执行
        canRun = false;
        // 7、定时器
        setTimeout( () => {
          fn.call(this, arguments);
          // 8、执行完事件（比如调用完接口）之后，重新将这个标志设置为 true
          canRun = true;
        }, 1000);
      };
    }

    // 3、需要节流的事件
    function sayThrottle() {
      console.log("节流成功！");
    }

  </script>
</body>
</html>
```



