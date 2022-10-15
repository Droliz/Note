# C ++

## C++基础

### 初识

#### 程序结构

```c++
#include <iostream>
using namespace std;
int main() {
	cout << "这是一个输出语句";
	system("pause");
	return 0;
}
```

#### 三字符组

三字符组就是用于表示另一个字符的三个字符序列，又称为三字符序列。三字符序列总是以两个问号开头。

三字符序列不太常见，但 C++ 标准允许把某些字符指定为三字符序列。以前为了表示键盘上没有的字符，这是必不可少的一种方法。

三字符序列可以出现在任何地方，包括字符串、字符序列、注释和预处理指令。

下面列出了最常用的三字符序列

|三字符组|替换|
|:--|:--:|
|??=|#|
|??/|\\|
|??'|^|
|??(|\[|
|??)|]|
|??!|\||
|??<|{|
|??>|}|
|??-|~|

#### 常量和变量

宏常量和const修饰变量都是不可改变的，宏常量定义在最上面，const修饰变量定义在函数内

宏常量用 `#define 常量名 值` 定义

`const` 关键字在定义变量时修饰即可，一旦加上const修饰，那么变量为常量不可修改

```c++
#include <iostream>
using namespace std;
// 宏常量
#define Day 7

int main()
{
	cout << "hello" << Day << endl;
	// const 修饰的变量
	const int days = 7;
	system("pause");
	return 0;
}
```



#### 关键字

![关键字](https://www.runoob.com/wp-content/uploads/2018/06/20130806104900234.jpg)
[关键字介绍](https://www.runoob.com/w3cnote/cpp-keyword-intro.html)

### 数据类型

#### 整型

|数据类型|占用空间|取值范围|
|:--:|:--|:--:|
|short（短整型）|2 字节|(-2^15 ~ 2^15 - 1)|
|int（整型）|4 字节|(-2^31 ~ 2^31 - 1)|
|long（长整型）|Windows为4字节，Linux为4字节（32位）、8字节（64位）|(-2^31 ~ 2^31 - 1)|
|long long（长长整型）|8 字节|(-2^63 ~ 2^63 - 1)|

#### sizeof 关键字

sizeof 关键字可以统计数据类型所占内存的大小`sizeof(数据类型 / 变量)`

```c++
#include <iostream>  
using namespace std;  
#define Day 7  
  
int main() {  
    cout << sizeof(int) << endl;  // 4
    cout << sizeof Day << endl;   // 4
    return 0;  
}
```

#### 实型（浮点型）

浮点型变量分为两种
* 单精度 float
* 双精度 double

|数据类型|占用空间|有效数字范围|
|:--:|:--:|:--|
|float|4字节|7位有效数字|
|double|8字节|15~16位有效数字|

```c++
int main() {  
  
    float f = 3.14f;  
    double d = 3.14;  
    cout << "f:" << f << endl;
    cout << "d:" << d << endl;  
    return 0;  
}
```

默认显示 6 位小数

#### 字符型

字符型变量只占用一个字节，字符型变量是将对应的 ASCII 编码放到存储单元

字符型在赋值时，不能使用双引号，只能使用单引号，而且单引号只能引用一个字符

```c++
int main() {  
  
    char ch = 'a';  
    cout << sizeof ch << endl;  // 1  
    return 0;  
}
```

#### 转义字符

常用的转义字符

|转义字符|含义|ASCII编码|
|:--:|:--|:--:|
|\\n|换行|010|
|\\t|水平制表（TAB）|009|
|\\\\|代表反斜杠|092|


#### 字符串类型

表示一串字符

c风格字符串：`char 变量名[] = "值"`  值必须用双引号

c++风格：`string 变量名 = "值"`

```c++
int main() {  
    char ch[] = "aasdas";    // 必须双引号
    string str = "123";  
    cout << ch << endl;  
    cout << str << endl;   
    return 0;  
}
```

#### 布尔类型

布尔类型只占一个字节大小

```c++
int main() {  
    bool flag = true;  
    cout << flag << endl;     // 1
    return 0;  
}
```


#### 数据的输入

通过关键字 `cin` 从键盘获取 `cin >> 变量1 >> 变量2 >> ....`


```c++
int main() {  
    int a, b;  
    cin >> a >> b;   // 将两个输入值按顺序赋值给 a b
    cout << a << b << endl;  
    return 0;  
}
```

### 运算符

#### 算术运算符

前置后置递增递减

前置的自增自减都是先计算自增自减，再赋值

后置的自增自减都是先赋值，再自增自减

```c++
int main() {  
    int a = 1, b = 10;  
    b = ++a;  // 先自增为 2 再赋值给 b    
    cout << a << " | " << b << endl;  // 2 | 2  
    a = 1;  
    b = 10;  
    b = a++; // 先赋值给 b 再自增  
    cout << a << " | " << b << endl;  // 2 | 1  
    return 0;  
}
```

#### 逻辑运算符

|运算符|意义|实例|结果|
|:--:|:--:|:--:|:--|
|!|非|!a|取反|
|&&|与|a && b|同为真即为真|
|\|\||或|a \|\| b|任意为真即为真|


### 程序流程结构

c++ 中支持最基本的三种程序运行结构
* 顺序结构
* 选择结构
	* if ... else
	* switch
* 循环结构
	* while
	* for
	* do ... while

**三目运算符**

简化基本的选择语句

```c++
int main() {  
    int a = 1, b = 10;  
    b = a > 0 ? 2 : 3;   // 条件 ? 为真时取值 : 为假时取值 
    return 0;  
}
```

**跳转语句**

一共有三种跳转语句：`break、continue、goto`

break和continue一般用于循环中用于终端或跳过循环

goto可以无条件的跳转到想要的语句

```c++
int main() {  
    cout << "1" << endl;  
    goto FLAG;  
    cout << "2" << endl;  
    cout << "3" << endl;  
    cout << "4" << endl;  
    FLAG:  
    cout << "5" << endl;  
    return 0;  
}

// 1
// 5
```

### 数组

#### 概述

数组就是一个集合，存储相同类型的数据，数组中元素的内存时连续的

#### 一维数组

一维数组的定义方式
* `数据类型 数组名[长度];`
* `数据类型 数组名[] = { 值 .... };`

```c++
int main() {  
    int arr[] = {1, 2, 3, 4, 5};  
    cout << "数组长度:" << sizeof(arr) / sizeof(arr[0]) << endl;  
    cout << "数组首地址:" << arr << endl;   // 内存地址
    return 0;  
}
```

数组名是一个常量，不能重新赋值


#### 二维数组

二维数组的定义
* `数据类型 数组名[行][列];`
* `数据类型 数组名[][列] = { 值 ... };`

```c++
int main() {  
    int arr[4][4];  
    cout << "地址：" << arr << endl;  
    cout << "行数：" << sizeof arr / sizeof arr[0] << endl;  
    cout << "列数：" << sizeof arr[0] / sizeof arr[0][0] << endl;  
}
```


### 函数

```c++
返回值类型 函数名 (参数) {
	// 代码块
	return ;  // 如果有返回值
}
```

#### 分文件编写

在头文件中写函数的声明，在源文件中编写函数的定义。通过导入头文件的方式，导入需要使用的函数

```cpp
//  test.h

#ifndef C___TEST_H  
#define C___TEST_H  
  
#endif //C___TEST_H  
  
int Max(int x, int y);   // 声明函数
```

```cpp
// test.cpp
#include "http://www.droliz.cn/h/test.h"    // 导入头文件
  
int Max(int x, int y) {   // 定义函数
    return x + y;  
}
```

```cpp
// main.cpp
#include <iostream>  
#include "h/test.h"    // 导入头文件
using namespace std;  
  
int main() {  
    int a, b;  
    a = 1;  
    b = 2;  
    int c = Max(a, b);  // 使用函数
    cout << c << endl;  
    return 0;  
}
```


### 指针

#### 概念

作用：可以通过指针简洁访问内存

内存编号是从 0 开始记录的，一般采用十六进制表示

可以利用指针变量保存地址

#### 指针定义

```cpp
int main() {  
    int a = 1;  
    int * p; // 定义指针  
    p = &a;  // 记录变量 a 的地址  
    cout << p << endl;  // 内存地址  
    cout << *p << endl;  // 解引用  值  
}
```

指针在 32 位系统占 4 字节，在 64 位占 8 字节

```cpp
int main() {  
    cout << sizeof(int *) << endl;  
    cout << sizeof(char *) << endl;  
    cout << sizeof(float *) << endl;  
    cout << sizeof(double *) << endl;  
}
```


#### 空指针

执政变量指向内存中编号为 0 的空间，用于初始化指针变量，空指针指向的内存时不可以访问的（0 ~ 255 为系统占用，不允许用户访问）

```cpp
int main() {  
    int * p = NULL;
    cout << p << endl;   // 0
    cout << *p << endl;  // error
}
```


#### 野指针

指针变量指向非法的内存空间

```cpp
int main() {
	// 野指针   避免野指针
	int * p = (int *)0x1100;
}
```

#### const 修饰指针

* const 修饰指针：常量指针
* const 修饰常量：指针常量
* const 即修饰常量又修饰指针

```cpp
int main() {  
    int a = 10;  
    int b = 20;  
  
    // 常量指针  
    const int * p1 = &a;  
    // 指针指向可以更改，但是指向的值不能更改  
    p1 = &b;  
    // *p1 = 20;  error  
  
    // 指针常量  
    int * const p2 = &a;  
    // 指针的值可以改，但是指针的指向不能改  
    *p2 = 30;  
    // p2 = &b;  error  
  
    // 同时修饰  
    const int * const p3 = &a;  
    // 指针的指向和值都不能改  
}
```

#### 指针访问数组

```cpp
int main() {  
    int arr[5] = {1, 2, 3, 4, 5};  
    int * p = arr;  
    cout << p << endl;  
  
    for (int i = 0; i < sizeof arr / sizeof arr[0]; i ++) {  
        cout << *p << endl;  
        p++;   // 数组中元素是连续的
    }  
    return 0;  
}
```

#### 指针和函数

```cpp
#include <iostream>  
using namespace std;  
  
// 值传递  
void swap1(int a, int b) {  
    int temp = a;  
    a = b;  
    b = temp;  
}  
  
// 地址传递  
void swap2(int * p1, int * p2) {  
    int temp = *p1;  
    *p1 = *p2;  
    *p2 = temp;  
}  
  
int main() {  
    int a = 1, b = 2;  
    // 值传递不会更改实参的值  
    swap1(a, b);  
    cout << a << " | " << b << endl;   // 1 | 2
    // 地址传递会更改实参的值  
    swap2(&a, &b);  
    cout << a << " | " << b << endl;   // 2 | 1
}
```

### 结构体

#### 概念

结构体属于用户自定义的数据类型，允许用户存储不同的数据类型

#### 定义和使用

使用关键字 `stryct` 结构体定义`stryct 结构体名 { 成员列表 }`

```cpp
// 定义结构体
struct Student {  
    string name;  
    int age;  
    string sex;  
};  
  
int main() {  
	// struct 可省略
    struct Student jack;  
    jack.name = "jack";  
    jack.age = 18;  
    jack.sex = "男";  
    cout << "name:" << jack.name << "age:" << jack.age << "sex:" << jack.sex << endl;  
    
    Student luci = {  
        "luci",  
        18,  
        "女"  
    };  
    cout << "name:" << luci.name << "age:" << luci.age << "sex:" << luci.sex << endl;  
}
```

#### 结构体数组

让数组中的每个元素的类型都是自定义的结构体

```cpp
int main() {  
	// 结构体数组
    struct Student arr[] = {  
        { "jack", 18, "男" },  
        { "luci", 18, "女" }  
    };  
}
```


#### 结构体指针

通过指针访问结构体中的成员：利用操作符 `->` 可以通过结构体指针访问结构体的属性

```cpp
int main() {  
    Student jack = { "jack", 18, "男" };  
    Student * p = &jack;  
    cout << p -> name << endl;    // 访问 name 属性
}
```

#### 结构体嵌套结构体

结构体的中的成员可以是另一个结构体

```cpp
#include <iostream>  
using namespace std;  
  
// 定义结构体  
struct Student {  
    string name;  
    int age;  
    string sex;  
};  
  
struct Teacher {  
    int id;  
    string name;  
    int age;  
    string sex;  
    struct Student student;  
};  
  
int main() {  
    Student jack = { "jack", 18, "男" };  
  
    Teacher Bob = {  
        001,  
        "Bob",  
        30,  
        "男",  
        jack,  
    };  
  
}
```


#### 结构体作为函数参数

* 通过值传递
* 通过地址传递

```cpp
// 值传递
void func(Student stu) {  
    cout << stu.name << endl;  
    cout << stu.age << endl;  
    cout << stu.sex << endl;  
}  

// 地址传递
void f(Student * p) {  
    cout << p -> name << endl;  
    cout << p -> age << endl;  
    cout << p -> sex << endl;  
}
```

#### 结构体中的 const

使用const限制，防止误操作

```cpp
// 限制
void f(const Student * p) {  
	// error
	p -> name = "aaa";  // 常量指针不能更改值
}
```


## C++ 核心编程

### 内存分区模型

C++程序在执行时，将内存大方向划分 4 个区域
* 代码区：存放函数体的二进制代码，有操作系统进行管理
* 全局区：存放全局变量和静态变量以及常量
* 栈区：有编译器自动分配释放，存放函数的参数值、局部变量等
* 堆区：有程序员分配和释放，若不释放，程序结束后由操作系统收回

不同区域存放的数据，赋予不同的生命周期，带来更大的灵活编程


##### 程序运行前

在程序编译后，生成 exe 可执行文件，未执行程序前分为两个区

**代码区**
* 存放 CPU 执行的机器指令
* 代码区是==共享==的，对于频繁被执行的程序，只需要在内存中由一份代码即可
* 代码区是==只读==的，防止程序意外的修改指令

**全局区**
* 全局变量和静态变量存放于此
* 全局区还包含了常量区，字符串常量和其他常量也存放于此
* ==该区域数据在程序结束后由操作系统释放==


##### 程序运行后

**栈区**
* 由编译器自动分配释放
* 注意：不要返回局部变量的地址，栈区开辟的数据由编译器自动释放

```cpp
int * func() {  
    int a = 10;    // 局部变量存放在栈区
    return &a;     // 函数执行完毕，栈区数据自动释放
}  
  
int main() {  
    int * p = func();   
    return 0;  
}
```

**堆区**
* 由开发人员分配脂肪，主要用 new 在堆区开辟内存

```cpp
int * func() {  
    int * a = new int(10);   // 在堆区开辟内存
    return a;  
}  
  
int main() {  
    int * p = func();  
    cout << *p << endl;   // 10
    return 0;  
}
```

#### new 操作符

C++中运用 new 操作符在堆区开辟数据，使用 delete 操作符释放

`new 数据类型`，`delete 地址`

new返回的是数据类型的指针
```cpp
int * p = new int();   // 0
delete p;  // 释放
string * p1 = new strint();  // "" 空字符串
delete p1;
int * arr = new int[10]   // 长度为 10 数组
delete[] arr;   // 释放数组需要加上 []
```


### 引用

#### 基本使用

作用：给变量起别名

语法：`数据类型 &别名 = 原名;`

```cpp
int main() {  
    int a = 10;  
    int &b = a;   // 引用必须初始化
    cout << "a=" << a << endl;  // 10  
    cout << "b=" << b << endl;  // 10  
    b = 20;  // 赋值
    cout << "a=" << a << endl;  // 20  
    cout << "b=" << b << endl;  // 20  
    return 0;  
}
```


引用不同于赋值，引用 a 、b 指向的还是同一块内存空间，而赋值是开辟了一块新的内存空间，故而 引用时，两个值是同一个

* 引用必须初始化
* 一旦初始化后，就不可以更改

#### 引用与函数

##### 函数参数

简化指针修改实参

```cpp
// 引用的形参可以用于修饰实参，最终实现更改实参
void func( int &a, int &b ) {  
    int temp = a;  
    a = b;  
    b = temp;  
}  
  
int main() {  
    int a = 10;  
    int b = 20;   
    func(a, b);    
    cout << a << endl;  
    cout << b << endl;  
}
```

##### 函数返回值

* 不返回局部变量的引用
* 函数调用可以作为左值

```cpp
int& func() {  
    static int a = 10;  // 静态变量，存放在全局区  
    return a;  
}  
  
int main() {  
    int &ref = func();   
    cout << ref << endl;  // 10  
    func() = 20;  // 作为左值  
    cout << ref << endl;  // 20  
    cout << func();  // 20
}
```

#### 引用本质

引用本质在 C++ 中内部实现就是一个 ==指针常量==

#### 常量引用

主要用于修饰形参，防止误操作，防止改变实参

```cpp
void func(const int &v) {
	v += 10;
	cout << v << endl;
}
```


### 函数提高

#### 参数

函数的参数可以有默认值，参数可以有占位参数（只写数据类型），占位参数在调用函数时必须填写值

#### 函数重载

##### 概念

满足函数重载条件
* 同一作用域
* 函数名相同
* 参数类型、个数、顺序不同（至少一个）

```cpp
int point (int x, int y) {  
    return x + y;  
}  
  
int point (int x, int y, int z) {  
    return x + y + z;  
}
```


##### 注意

* 引用作为重载条件
* 函数重载与默认参数

```cpp
// 以引用作为条件
int point (int &x) {    // 参数是变量时调用
    return x;  
}  
  
int point (const int &x) {    // 参数是常量是调用
    return x;  
}


// 函数重载与默认参数
int point (int x, int y = 10) {  
    return x;  
}  
  
int point (int x) {  
    return x;  
}  
  
int main() {  
    point(1);   // error 两个函数都符合调用，无法确定调用哪一个，二义性
}
```


### 类和对象

#### 概念

C++认为==万物皆对象==，C++的面向对象三大特性：==封装、继承、多态==

#### 封装

将属性和行为作为一个整体

```cpp
#include <iostream>  
using namespace std;  
#include "string"  
  
class Person {  
public:   // 访问修饰符  
    void eat();    // 声明成员函数  
    void set(string n, string s, int a);  
    string getName();  
private:  
    string name;   // 变量  
    string sex;  
    int age;  
};  
  
// 定义成员函数  可以在声明的时候定义  
void Person::eat() {  
    cout << "eat" << endl;  
}  
  
void Person::set(string n, string s, int a) {  
    name = n;  
    sex = s;  
    age = a;  
}  
  
string Person::getName() {  
    return name;  
}  
  
int main() {  
    Person jack;   // 实例化对象  
    // 调用方法  
    jack.set("jack", "男", 18);  
    cout << jack.getName() << endl;  
}
```

属性修饰符：`public、private、protected`

私有权限仅类可以使用，保护权限不仅仅类可以，类的子类也可以访问

class默认是私有权限，struct默认为公有权限

一般的有些属性需要私有化，然后通过公有的方法进行赋值，获取等操作

#### 对象的初始化和清理

##### 构造函数和析构函数

* 构造函数：主要用于创建对象是为成员属性赋值（创建对象后自动调用，仅一次）
* 析构函数：执行一些清理工作（对象销毁前自动调用，仅一次）

```cpp
// 构造函数
类名() {   // 可以有参数（可重载）  无需返回值也不写 void

}

// 析构函数
~类名() {  // 不可以有参数   无需返回值也不写 void

}
```

```cpp
#include <iostream>  
using namespace std;  
#include "string"  
  
class Person {  
public:   // 访问修饰符  
    void eat();    // 声明成员函数  
    void set(string n, string s, int a);  
    string getName();  
    Person();  // 构造函数  
    ~Person(); // 析构函数  
  
private:  
    string name;   // 变量  
    string sex;  
    int age;  
};  
  
// 定义成员函数  可以在声明的时候定义  
void Person::eat() {  
    cout << "eat" << endl;  
}  
  
void Person::set(string n, string s, int a) {  
    name = n;  
    sex = s;  
    age = a;  
}  
  
string Person::getName() {  
    return name;  
}  
  
Person::Person() {  
    cout << "构造函数" << endl;  
    // 初始化  
    name = "";  
    sex = "";  
    age = 0;  
}  
  
Person::~Person() {  
    cout << "析构函数" << endl;  
}  
  
  
int main() {  
    Person jack;   // 实例化对象  
    // 调用方法  
    jack.set("jack", "男", 18);  
    cout << jack.getName() << endl;  
}

// 构造函数
// jack
// 析构函数
```

当构造函数有参数时，需要在实例化时传入 `Person jack(value ... )`

##### 构造函数的分类及调用

两种分类方式
* 按参数：有参和无参
* 按类型：普通和拷贝
	```cpp
	class Person {  
	public:     
	    Person(){  
	        cout << "构造函数" << endl;  
	    }  
	    // 拷贝构造函数  
	    Person(const Person &p) {   // 限制不能改变，以引用方式传入  
		    属性名 = p.属性名    // 将 p 的属性赋值到此实例
		    // age = p.age; 
	        cout << "拷贝构造函数" << endl;  
	    }  
	};
	```

三种调用方式
* 括号法
	```cpp
	Person p1;              // 默认构造函数调用
	Person p2(value ... );  // 有参构造函数调用
	Person p3(p2);          // 拷贝构造函数   传入其他的类实例 p2

	// 默认的调用不加 ()，否则编译器会认为是函数的声明
	```
* 显示法
	```cpp
	Person p2 = Person(value ...);   // 有参构造
	Person p3 = Person(p2);          // 拷贝构造

	// 右值为匿名对象，当执行完，系统会立即回收匿名对象，故需要赋值
	```
	不要用拷贝构造函数，初始化匿名对象，编译器会将 `Person(p2) === Person p2`，就会认为 p2 重定义
* 隐式转换法
	```cpp
	Person p2 = value;   // 相当于 Person p2 = Person(value);
	Person p3 = p2;      // 拷贝构造
	```

##### 拷贝构造函数调用时机

三种情况
* 使用一个已经创建的对象来初始化一个新对象
* 值传递的方式给函数参数传值
* 以值返回方式返回局部对象

##### 构造函数调用规则

默认情况下 C++ 会给一个类添加三个函数
* 默认构造函数（无参，函数体为空）
* 默认析构函数（无参，函数体为空）
* 默认拷贝构造函数（对属性值进行拷贝）

如果用户定义有参构造函数，那么 C++ 不提供默认无参构造函数，但是会提供默认拷贝构造函数。如果用户定义拷贝构造函数，C++ 不会提供其他构造函数

##### 深拷贝与浅拷贝

浅拷贝：简单的赋值拷贝操作
深拷贝：在堆区重新申请空间，进行拷贝操作

```cpp
#include <iostream>  
using namespace std;  
  
class Person {  
public:  
    Person(int age, int height);  
    ~Person();  
    int mAge;  
    int *mHeight;   // 存放到堆区  
};  
  
Person::Person(int age, int height) {  
    cout << "有参构造函数!" << endl;  
    mAge = age;   // 直接赋值  
    mHeight = new int(height);  // 需要在堆区开辟空间  
}  
  
Person::~Person() {  
    if (mHeight != NULL) {  
        // 释放堆区数据  
        delete mHeight;  
        // 防止野指针  
        mHeight = NULL;  
    }  
    cout << "析构函数!" << endl;  
}  
  
int main() {  
    Person jack(18, 175);  
    Person bob(jack);   // 编译器提供的拷贝构造函数为浅拷贝  
    // 浅拷贝会导致堆区的内存重复释放  
  
    cout << jack.mHeight<< endl;  
    cout << bob.mHeight << endl;   // 地址相同  
    return 0;  
}
```

由于默认提供的拷贝构造函数提供的是浅拷贝，所以导致会重复释放内存

需要自己写可瓯北构造函数

```cpp
Person (const Person &p) {
	mAge = p.mAge;
	// mHeight = p.mHeight;   默认，浅拷贝
	mHeight = new int(p.mHeight);   // 深拷贝
}
```

##### 初始化列表

C++ 提供了初始化列表语法，用来初始化属性

语法：`构造函数(): 属性(值), 属性(值) ....`

```cpp
class Person {  
public:  
    Person(): mAge(18), mHeight(190) {
	    // code
    };  
    int mAge;  
    int mHeight;  
};
```

##### 类对象作为类成员

C++类成员可以是另一个类对象，称为成员对象

```cpp
class A {}
class B {
	A a;   // 对象成员
}
```

上述代码中，执行顺序：A 构造函数 -> B 构造函数 -> B 析构函数 -> A 析构函数

```cpp
#include <iostream>  
using namespace std;  
  
class Phone {  
public:  
    Phone(string pName) {  
        name = move(pName);  
    };  
    string name;  
};  
  
class Person {  
public:  
    Person(string name, string pName): mName(move(name)), mPhone(move(pName)){};  
    string mName;  
    Phone mPhone;  
};  
  
int main() {  
    Person jack("jack", "iPhone");  
    cout << jack.mPhone.name << endl;  
}
```

C++11的标准库 `<utility>` 提供了一个非常有用的函数 `std::move()`，std::move() 函数将一个左值强制转化为右值引用，以用于移动语义。

移动语义，允许直接转移对象的资产和属性的所有权，而在参数为右值时无需复制它们。

换一种说法就是，std::move() 将对象的状态或者所有权从一个对象转移到另一个对象，只是转移，没有内存的搬迁或者内存拷贝。

因此，通过std::move()，可以避免不必要的拷贝操作。

##### 静态成员

使用 `static` 关键字

静态成员变量
* 所有对象共享一份数据
* 在编译阶段分配内存
* 类内声明，类外初始化
静态成员函数
* 所有的对象共享同一个函数
* 静态成员函数只能访问静态成员变量

静态成员变量
```cpp
class Person {  
public:  
    static string mName;  
};  

// 类外初始化
string Person::mName = "jack";

int main() {  
    Person p;  
    cout << Person::mName << endl;    // 通过类访问
    cout << p.mName << endl;          // 通过对象访问
}
```

静态成员函数
```cpp
class Person {  
public:  
    static void func();  
};  
  
void Person::func() {  
    cout << "静态函数" << endl;  
}  
  
  
int main() {  
    Person p;  
    Person::func();   // 类调用
	p.func();         // 对象调用
}
```

#### C++ 对象模型和this指针

##### 成员变量和成员函数的分开存储

在C++中，类内的成员变量和成员函数分开存储，==只有非静态成员变量才属于类的对象上==

```cpp
class Person {  
public:  
    Person();   
    void func();   // 函数不占对象空间，所有函数共享  
    static int mId;  // 静态成员变量不占对象空间  
    int mAge;     // 非静态成员变量占对象空间  
};  
  
int Person::mId = 1;  
  
void Person::func() {  
    cout << "func" << endl;  
}  
  
Person::Person() {  
    mAge = 0;  
}
```

c++编译器对于空对象会分配一个字节空间 `class A {}`

##### this指针

==this指针指向被调用的成员函数所属的对象==

this指针时隐含每一个非静态成员函数内的一种指针，不需要定义，直接使用

用途：
* 当形参和成员变量同名时，可以使用this指针来区分
* 在类的非静态成员函数中返回对象本身，可以使用 `return *this;`

```cpp
class Person {  
public:  
    Person(int age);  
    int age;  
    Person& func();  
};  
  
Person::Person(int age) {  
    this -> age = age;   // this 区分  
}  
  
Person& Person::func() {  
    return *this;   // 返回调用此方法的对象
}  
  
  
int main() {  
    Person p(18);  
    cout << p.age << endl;  // 18  
    Person p1 = p.func();    // Person p1 = Person(p) 
    cout << p1.age << endl;  // 18  
}
```

##### 空指针访问成员函数

C++ 允许空指针调用成员函数，但是需要注意有没有用到this指针

如果用到 this 指针，需要加以判断保证代码的健壮性

```cpp
class Person {  
public:  
    void showCN() {  
        cout << "Person" << endl;  
    }  
  
    void showAge() {  
        // 防止传空指针，导致无法访问变量  
        if (this == NULL) {  
            return ;  
        }  
        cout << age << endl;   // 默认是 this -> age    }  
  
    int age;  
};  
  
void test() {  
    Person *p = NULL;  
    p->showAge();  
    p->showCN();  
}  
  
int main() {  
    test();  
    return 0;  
}
```

##### const 修饰成员函数

常函数：
* 不可以修改成员属性
* 给成员属性添加关键字 `mutable` 后，常函数可以修改

常对象
* 只能调用常函数

```cpp
class Person {  
public:  
    Person(int age) {  
        this->age = age;  
    }  
    void add() const {   // 本质是指针常量，让指针的值不可以修改  
        this->age += 1;  
    }  
  
    mutable int age;  // 特殊变量  
    int height;
};  
  
int main() {  
    Person p(1);  
    p.add();  
    cout << p.age << endl;   // 2  
  
    // 常对象  
    const Person p1(2);  
    p1.add();    // 常对象只能调用常函数  
    cout << p1.age << endl;   // 3  
    p1.height = 180   // error，不允许更改普通的变量
    return 0;  
}
```

#### 友元

##### 概念

在程序中，有些私有属性也想让类外特殊的函数或类访问，就需要友元

使用关键字 `friend`

友元的三种实现
* 全局函数做友元
* 类做友元
* 成员函数做友元

##### 全局函数做友元

如果要声明函数为一个类的友元，需要在类定义中该函数原型前使用关键字 **friend**，如下所示：

```cpp
```cpp
class Person {  
    // 声明友元函数 getAge()
    friend void getAge(Person &p);  
    
public:  
    Person(string name, int age) {  
        this->name = move(name);  
        this->age = age;  
    }  
public:  
    string name;  
private:  
    int age;  
};

// 定义全局函数  
void getAge(Person &p) {  
    cout << p.age << endl;  
}  
  
int main() {  
    Person jack("jack", 18);  
    getAge(jack);  
    return 0;  
}
```

##### 类做友元

声明类 ClassTwo 的所有成员函数作为类 ClassOne 的友元，需要在类 ClassOne 的定义中放置如下声明：

```cpp
friend class ClassTwo;
```

```cpp
class Home;  
class Person;  
  
class Person {  
public:  
    Person();  
    Home * home;  
    void visit();  
};  

// 类 Person 的所有成员函数作为 Home 的友元
class Home {  
    friend class Person;  
public:  
    Home(string name, int width, int height);  
    string name;  
  
private:  
    int width;  
    int height;  
};  

Person::Person() {  
    home = new Home("home", 100, 100);  
}  

// 访问其他类的私有属性
void Person::visit() {  
    cout << "height:" << home->height << endl;  
    cout << "width:" << home->width << endl;  
}  
  
Home::Home(string name, int width, int height) {  
    this->name = move(name);  
    this->width = width;  
    this->height = height;  
}  
  
int main() {  
    Person jack;  
    jack.visit();  
  
    return 0;  
}
```

##### 成员函数做友元

类似于全局函数做友元

```cpp
friend 返回值类型 类名::成员函数();
```

在类做友元中更改 `friend class Person;` 为 `friend void Person::visit();` 即可


#### 运算符重载

##### 概念

对已有的运算符重新进行定义，赋予另一种功能，以适应不同的数据类型

编译器使用关键字`operator`提供了特殊的函数名：`operator运算符` 来实现不同运算符的重载

##### 加号运算符重载

成员函数
```cpp
#include <iostream>  
using namespace std;  
  
class Person {  
public:  
    Person(int age, int uid);  
    int age;  
    int uid;  
    // 成员函数重载    限制
    Person operator+(Person &p) const;  
};  
  
Person::Person(int age, int uid)  {  
    this->age = age;  
    this->uid = uid;  
}  
  
Person Person::operator+(Person &p) const {  
    Person temp(0, 0);  
    temp.age = this->age + p.age;  
    temp.uid = this->uid + p.uid;  
    return temp;  
}  
  
int main() {  
    Person p1(10,10);  
    Person p2(20, 20);  
	// 本质 p2 = p1.operator+(p2);
    p2 = p1 + p2;   // 不支持 +=    cout << "age:" << p2.age << endl;   // 30  
    cout << "uid:" << p2.uid << endl;   // 30  
    return 0;  
}
```


全局函数
```cpp
Person operator+(Person &p1, Person &p2) {  
    Person temp(0, 0);  
    temp.age = p1.age + p2.age;  
    temp.uid = p1.uid + p2.uid;  
    return temp;  
}

int main() {
	p3 = p1 + p2 // 本质 p3 = operator+(p1, p2);
}
```

##### 左移运算符重载

作用：输出自定义数据类型

只能用全局函数重载

```cpp
class Person {  
public:  
    Person(int age, int uid);  
    int age;  
    int uid;  
};  
  
Person::Person(int age, int uid)  {  
    this->age = age;  
    this->uid = uid;  
}  
  
// 本质 operator<<(out, p) 简化 out << p
ostream & operator<<(ostream &out, Person &p) {  
    out << "age=" << p.age << "uid=" << p.uid;  
    return out;    // 输出 out 符合链式，以达到后面继续拼接 << endl; 等
};  
  
  
int main() {  
    Person jack(18, 1);  
    cout << jack << endl;  
    return 0;  
}
```

##### 可重载运算符

|运算符|种类|
|:--|:--|
|双目运算符|+ (加)，-(减)，\*(乘)，/(除)，% (取模)|
|关系运算符|\==(等于)，!= (不等于)，< (小于)，> (大于)，<=(小于等于)，>=(大于等于)|
|逻辑运算符|\|\|(逻辑或)，&&(逻辑与)，!(逻辑非)|
|单目运算符|+ (正)，-(负)，\*(指针)，&(取地址)|
|自增自减运算符|++(自增)，--(自减)|
|位运算符|\| (按位或)，& (按位与)，~(按位取反)，^(按位异或),，<< (左移)，>>(右移)|
|赋值运算符|=, +=, -=, \*=, /= , % = , &=, |=, ^=, <<=, >>=|
|空间申请与释放|new, delete, new[ ] , delete[]|
|其他运算符|()(函数调用)，->(成员访问)，,(逗号)，\[](下标)|

[c++重载运算符和重载函数](https://www.runoob.com/cplusplus/cpp-overloading.html)

#### 继承

##### 概念

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220814062636.png)

有些类之间有着特殊的关系，例如动物有的行为和外观，猫等都有

此时可以使用继承

##### 继承的使用

语法：
```cpp
// 基类  
class Animal {  
    // eat() 函数  
    // sleep() 函数  
};  
  
  
//派生类  
class Dog : public Animal {  
    // bark() 函数  
};
```


```cpp
  
// 基类  
class Shape {  
public:  
    void setWidth(int w) {  
        width = w;  
    }  
  
    void setHeight(int h) {  
        height = h;  
    }  
  
protected:  
    int width;  
    int height;  
};  
  
// 派生类  
class Rectangle : public Shape {  
public:  
    int getArea() {  
        return (width * height);  
    }  
};  
  
int main() {  
    Rectangle Rect;  
  
    Rect.setWidth(5);  
    Rect.setHeight(7);  
  
    // 输出对象的面积  
    cout << "Total area: " << Rect.getArea() << endl;  // 35  
  
    return 0;  
}
```

##### 继承方式

继承：`class 派生类 : 继承方式 基类 {};`

继承方式
* 公有继承
* 保护继承
* 私有继承

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220814064958.png)


##### 继承中的对象模型

子类继承父类也会继承私有属性，只是不能访问到，故而计算对象的sizeof时也要计算上私有属性的大小

```cpp
  
class F {  
public:  
    int a;  
private:  
    int b;  
protected:  
    int c;  
};  
  
class S: public F {  
public:  
    int d;  
};  
  
int main() {  
    S s;  
    cout << sizeof s << endl;  // 16  
    return 0;  
}
```

##### 继承中的构造和析构顺序

```cpp
#include<iostream>
#include<string>
using namespace std;


class base//父类
{
	
public:
	base()
	{
		cout << "父类的构造" << endl;
	}

	~base()
	{
		cout << "父类的析构" << endl;
	}
	  int a;
protected:
	
	  int b;
private:
	
	  int c;
};


/*公有继承*/
class son1:public base  
{
public:

	son1()
	{
		cout << "子类的构造" << endl;
	}

	~son1()
	{
		cout << "子类的析构" << endl;
	}

	int d;
};



int main()
{
	son1 s;
	
}

// 父类的构造
// 子类的构造
// 子类的析构
// 父类的析构
```

父类构造 -> 子类构造 -> 子类析构 -> 父类析构

删除子类对象，会先调用子类的析构函数

##### 继承同名成员处理方式

当子类于父类出现同名成员，通过子类对象访问子类或父类中的同名数据

* 访问子类同名成员  直接访问
* 访问父类同名成员  需加载作用域

```cpp
class F {  
public:  
    F() {  
        a = 100;  
    }  
    int a;  
};  
  
class S : public F {  
public:  
    S() {  
        a = 200;  
    }  
    int a;  
};  
  
int main() {  
    S son;  
    cout << son.a << endl;   // 200  
    cout << son.F::a << endl;  // 100  
    return 0;  
}
```

##### 继承同名静态成员处理方法

与非静态成员一致 [继承同名成员处理方式](#继承同名成员处理方式)

子类的同名函数会隐藏父类的成员函数

##### 多继承

语法：`class 子类 : 继承方式 父类, 继承方式 父类 ....`

多继承可能会引发父类中有同名成员出现，需要加作用域区分

##### 棱形继承

两个子类同时继承一个父类，又有一个子类，同时继承这两个类

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220814072613.png)

羊继承了动物的数据，驼也继承了动物数据，羊驼在使用数据时就会产生二义性

羊驼继承来源于动物的数据两份，事实上只需要一份

```cpp
class Animal {  
public:  
    int age;  
};

// 在 public 前添加 virtual 关键字，表示虚继承， 此时 Animal 为虚基类
class Sheep: virtual public Animal {  
  
};  
  
class Tuo: virtual public Animal {};  
  
class YT: public Sheep, public Tuo {};  
  
int main() {  
    YT yt;  
    yt.Sheep::age = 18;    // 如果不采用虚继承，yt 对象包含两份数据
    yt.Tuo::age = 28;  
  
    cout << yt.Sheep::age << endl;  // 28  
    cout << yt.Tuo::age << endl;    // 28  
  
    return 0;  
}
```

上述代码中，虚继承之后，YT继承会继承两个指针，通过偏移量找到唯一记录的数据


#### 多态

##### 概念

多态分为两类
* 静态多态：函数重载和运算符重载属于静态多态
* 动态多态：派生类和虚函数实现运行时多态

区别
* 静态多态的函数地址是早绑定的 —— 编译阶段确定函数地址
* 动态多态的函数地址是晚绑定的 —— 运行阶段确定函数地址


**动态多态满足条件**
* 1、有继承关系
* 2、子类要重写父类的虚函数

**动态多态的使用**
父类的指针或引用指向子类对象

```cpp
#include<iostream>  
using namespace std;  
  
class Animal {  
public:  
    void speak();    // 没有使用虚函数，地址早绑定
};  
  
void Animal::speak() {  
    cout << "Animal speak" << endl;  
}  
  
class Cat : public Animal { 
public:  
    void speak();  
};  
  
void Cat::speak() {  
    cout << "Cat speak" << endl;  
}  

void doSpeak(Animal &animal) {   
    animal.speak();    // 如果指向是虚函数，那么在编译阶段无法绑定函数
}  
  
int main() {  
    Cat cat;  
    doSpeak(cat);    // Animal speak
    return 0;  
}
```

在上述代码中，在编译阶段就确定了函数地址，但是不符合需要绑定 Cat 中 Speak，那么在Animal中的speak就需要是虚函数

```cpp
class Animal {  
public:  
    virtual void speak();    // 虚函数
};
```

speak 函数是根据对象不同执行不同的 speak，所以doSpeak函数的 speak 地址不能提前确定

在上述代码中，一旦 Animal 的 speak 为虚函数，那么 Animal 占用空间为 4 字节，在类中多了一个指针 `vfptr` （virtual function pointer）==虚函数（表）指针==，指针指向虚函数表 `vftable`，表内记录虚函数地址

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220814170855.png)
==Cat 中继承的 &Animal::speak 会被重写的 speak 覆盖==，Cat 中重写时 virtual 可写可不写

**多态优点**
* 代码组织结构清晰
* 可读性强
* 利于前后期的扩展以及维护

##### 案例——计算器

普通实现计算器类
```cpp
 class Calculator {  
public:  
    Calculator(int a, int b);  
    int a, b;  
    int getResult(char oper);  
  
};  
  
Calculator::Calculator(int a, int b) {  
    this->a = a;  
    this->b = b;  
}  
  
int Calculator::getResult(char oper) {  
    switch (oper) {  
        case '+': return a + b;  
        case '-': return a - b;  
        case '*': return a * b;  
        case '/': return a / b;  
        case '%': return a % b;  
        default: break;  
    }  
    return 0;  
}
```

如果想扩展新的功能，需要更改 getResult 成员方法。开发中提放开闭原则：对扩展进行开放，对修改进行关闭

多态实现计算器
```cpp
// 实现计算器的抽象类  
class AbstractCalculator {  
public:  
    int a, b;  
    virtual int getResult() {  
        return 0;  
    }  
};  
  
// 加法类  
class Add : public AbstractCalculator {  
public:  
    int getResult();  
};  
  
int Add::getResult() {  
    return a + b;  
}  
  
// 减法  
class func : public AbstractCalculator {  
public:  
    int getResult();  
};  
  
int func::getResult() {  
    return a - b;  
}  
  
int main() {  
    // 父类指针指向子类对象  
    AbstractCalculator * abs;  
    abs = new Add;  
    abs->a = 10;  
    abs->b = 10;  
  
    cout << abs->getResult() << endl;    // 20  
    delete abs;   // 销毁  
  
    abs = new func;  
    abs->a = 10;  
    abs->b = 10;  
  
    cout << abs->getResult() << endl;    // 0  
    delete abs;  
  
    // 父类别名指向子类对象  
    AbstractCalculator &ABS = * new Add;  
  
    ABS.a = 10;  
    ABS.b = 10;  
    cout << ABS.getResult() << endl;  // 20  
    delete &ABS;  
    return 0;  
}
```

如果有需要只用在后面创建类继承计算器类即可，使用时，将父类指针或别名指向子类对象即可


##### 纯虚函数与抽象类

在多态中，父类中的虚函数往往没有作用，都需要子类重写，可以将此部分改为纯虚函数 

```cpp
// 纯虚函数
virtual 返回值类型 函数名 (参数列表) = 0;
```

当类中有纯虚函数，那么这个类也叫做抽象类

**抽象类特点**
* 无法实例化对象
* 子类必须重写朝向类中的纯虚函数，否则子类也属于抽象类

```cpp
// 实现计算器的抽象类  
class AbstractCalculator {  
public:  
    int a, b;  
    virtual int getResult() = 0;  
};  
  
// 加法类  
class Add : public AbstractCalculator {  
public:  
    int getResult() override;  
};  
  
int Add::getResult() {  
    return a + b;  
}  
  
// 减法  
class func : public AbstractCalculator {  
public:  
    int getResult() override;  
};  
  
int func::getResult() {  
    return a - b;  
}  
  
  
int test(AbstractCalculator &abs) {  
    abs.a = 10;  
    abs.b = 10;  
    return abs.getResult();  
}  
  
int main() {  
    cout << test(*new Add);  
}
```


##### 虚析构和纯虚析构

在使用多态时，如果子类中有属性开辟到堆区，那么父类指针在释放时无法调用到子类的析构代码，会造成内存泄漏

解决：在父类中将析构函数改为虚析构或纯虚析构

==不论是虚析构函数纯虚析构都需要具体的实现==

```cpp
class Animal {  
public:  
    Animal();  
    ~Animal();  
    virtual void speak() = 0;  
};  
  
Animal::Animal() {  
    cout << "Animal 构造" << endl;  
}  
  
Animal::~Animal() {  
    cout << "Animal 析构" << endl;  
}  
  
class Cat : public Animal {  
public:  
    Cat();  
    void speak();  
    string *Name;   // 用指针维护 创建在堆区  
    ~Cat();  
};  
  
void Cat::speak() {  
    cout << "speak " << *Name << endl;  
}  
  
Cat::Cat() {  
    cout << "cat 构造" << endl;  
    Name = new string("cat");  
}  
  
Cat::~Cat() {  
    if (Name != NULL) {  
        cout << "cat 析构" << endl;  
        delete Name;  
        Name = NULL;  
    }  
}  
  
int main() {  
    Animal * animal = new Cat;    // 父类的指针在析构时不调用子类析构，如果有堆区属性，会导致内存泄漏
    animal->speak();  
    delete animal;  
    return 0;  
}

// 不会调用 cat 析构去释放
// Animal 构造
// cat 构造
// speak cat
// Animal 析构
```


上述如果改为纯虚析构必须在类外面写上具体的代码的实现
```cpp
class Animal {  
public:  
    Animal();  
    virtual ~Animal() = 0;  
    virtual void speak() = 0;  
};  
  
Animal::Animal() {  
    cout << "Animal 构造" << endl;  
}  
  
Animal::~Animal() {  
    cout << "Animal 析构" << endl;    // 可以不写任何代码块，但是必须有
}
```

只有在子类中有属性开辟在堆区，需要使用子类的析构函数释放内存，才需要使用虚析构或纯虚析构


##### 实例——计算机

计算机包含：`CPU、显卡、内存条`

CPU、显卡、内存条都是抽象类，需要具体不同厂商子类

计算机类再传入不同部件的指针，通过指针调用不同类下的成员函数

```cpp
#include<iostream>  
using namespace std;  

// 抽象类
class CPU {  
public:  
    virtual void caculate() = 0;  
};  
  
class Display {  
public:  
    virtual void show() = 0;  
};  
  
class RAM {  
public:  
    virtual void save() = 0;  
};  
  
class InterCPU : public CPU {  
public:  
    void caculate() override;   // override 保留字表示当前函数重写了基类的虚函数。
};  
  
void InterCPU::caculate() {  
    cout << "Inter  CPU" << endl;  
}  
  
class SancDisplay : public Display {  
public:  
    void show() override;  
};  
  
void SancDisplay::show() {  
    cout << "SANC 显示器" << endl;  
}  
  
class neicunRAM : public RAM {  
public:  
    void save() override;  
};  
  
void neicunRAM::save() {  
    cout << "内存" << endl;  
}  

// 计算机类
class Comput {  
public:  
    Comput(CPU *cpu, Display *display, RAM *ram);  
    ~Comput();  
    void make();  
private:  
    CPU *cpu;  
    Display *display1;  
    RAM *ram;  
};  
  
Comput::Comput(CPU *cpu, Display *display, RAM *ram) {  
    cout << "组装" << endl;  
    this->cpu = cpu;  
    this->display1 = display;  
    this->ram = ram;  
}  
  
Comput::~Comput() {  
    if (cpu != NULL && display1 != NULL && ram != NULL) {  
        cout << "释放" << endl;  
        delete cpu;  
        delete display1;  
        delete ram;  
        cpu = NULL;  
        display1 = NULL;  
        ram = NULL;  
    }  
}  
  
void Comput::make() {  
    cpu->caculate();  
    display1->show();  
    ram->save();  
}  
  
  
int main() {  

	Comput * comput = new Comput(new InterCPU, new SancDisplay, new neicunRAM);  
	comput->make();  
	delete comput;
    return 0;  
}


// 组装
// Inter  CPU
// SANC 显示器
// 内存
// 释放
```


#### C++存储类

存储类定义 C++ 程序中变量/函数的范围（可见性）和生命周期。这些说明符放置在它们所修饰的类型之前。下面列出 C++ 程序中可用的存储类

-   auto
-   register
-   static
-   extern
-   mutable
-   thread_local (C++11)

从 C++ 17 开始，auto 关键字不再是 C++ 存储类说明符，且 register 关键字被弃用

##### auto存储类

自 C++ 11 以来，**auto** 关键字用于两种情况：声明变量时根据初始化表达式自动推断该变量的类型、声明函数时函数返回值的占位符。

C++98标准中auto关键字用于自动变量的声明，但由于使用极少且多余，在 C++17 中已删除这一用法

```cpp
auto f=3.14; //double 
auto s("hello"); //const char* 
auto z = new auto(9); // int* 
auto x1 = 5, x2 = 5.0, x3='r';//错误，必须是初始化为同一类型
```

##### register存储类

**register** 存储类用于定义存储在寄存器中而不是 RAM 中的局部变量。这意味着变量的最大尺寸等于寄存器的大小（通常是一个词），且不能对它应用一元的 '&' 运算符（因为它没有内存位置）

```cpp
{ 
register int miles; 
}
```

##### static存储类

**static** 存储类指示编译器在程序的生命周期内保持局部变量的存在，而不需要在每次它进入和离开作用域时进行创建和销毁。因此，使用 static 修饰局部变量可以在函数调用之间保持局部变量的值。

static 修饰符也可以应用于全局变量。当 static 修饰全局变量时，会使变量的作用域限制在声明它的文件内。

在 C++ 中，当 static 用在类数据成员上时，会导致仅有一个该成员的副本被类的所有对象共享

```cpp
#include <iostream> 

// 函数声明 
void func(void); 
static int count = 10; /* 全局变量 */ 
int main() { 
	while(count--) { 
		func(); 
	} 
	return 0; 
} 

// 函数定义 
void func( void ) { 
	static int i = 5; // 局部静态变量 
	i++; 
	std::cout << "变量 i 为 " << i ; 
	std::cout << " , 变量 count 为 " << count << std::endl; 
}
```

##### extern存储类

**extern** 存储类用于提供一个全局变量的引用，全局变量对所有的程序文件都是可见的。当您使用 'extern' 时，对于无法初始化的变量，会把变量名指向一个之前定义过的存储位置

extern 修饰符通常用于当有两个或多个文件共享相同的全局变量或函数的时候

```cpp
//main.cpp
#include <iostream> 
int count; 
extern void write_extern(); 
int main() { 
	count = 5; 
	write_extern(); 
}

// support.cpp
#include <iostream> 
extern int count; 
void write_extern(void) { 
	std::cout << "Count is " << count << std::endl; 
}
```

##### mutable存储类

它允许对象的成员替代常量。也就是说，mutable 成员可以通过 const 成员函数修改

##### thread_local存储类

使用 thread_local 说明符声明的变量仅可在它在其上创建的线程上访问

thread_local 说明符可以与 static 或 extern 合并

```cpp
thread_local int x; // 命名空间下的全局变量 

class X { 
	static thread_local std::string s; // 类的static成员变量 
}; 

static thread_local std::string X::s; // X::s 是需要定义的 

void foo() { 
	thread_local std::vector<int> v; // 本地变量 
}
```

### 文件操作

#### 概述

再程序运行时产生的数据都属于临时文件，运行完都会被释放

C++中对文件操作需要包含头文件 `<fstream>`

文件类型分为两种
* 文本文件     ——   文件以文本的 ASCII 码形式存储再计算机中
* 二进制文件 ——   文件以文本的二进制形式存储再计算机中

操作文件的三大类
* ofstream：写操作
* ifstream：读操作
* fstream：读写操作


#### 文本文件

##### 写文件

步骤
* 包含头文件
	```cpp
	#include <fstream>
	```
* 创建流对象
	```cpp
	ofstream ofs;
	```
* 打开文件
	```cpp
	ofs.open("文件路径", 打开方式);
	```
* 写数据
	```cpp
	ofs << "写入数据";
	```
* 关闭文件
	```cpp
	ofs.close();
	```


文件打开方式

|打开方式|说明|
|:--|:--|
|ios::in|只读|
|ios::out|只写|
|ios::ate|初始位置：文件末尾|
|ios::app|以追加写入|
|ios::trunc|如果有文件，先删除|
|ios::binary|以二进制打开|

文件打开方式可以用操作符 `|` 配置选择多个


```cpp
int main() {  
    string arr[] = {"jack", "18", "男"};  
    ofstream ofs;  
  
    ofs.open(R"(F:\vs_code\c++\test.txt))", ios::out);  
    if (!ofs) {  
        cout << "error" << endl;  
        return 0;  
    }  
    for (auto & i : arr) {  
        ofs << i << endl;  
    }  
    ofs.close();  
}
```

##### 读文件

```cpp
#include<iostream>  
using namespace std;  
#include<fstream>  
  
void test() {  
  
    ifstream ifs;  
  
    ifs.open(R"(F:\vs_code\c++\test.txt)", ios::out);  
    if (!ifs.is_open()) {  
        cout << "失败" << endl;  
        return ;  
    }  
    
    // 读取数据  
    char buf[1024] = {0};  
    while ( ifs >> buf ) {   // 以字符读取  
        cout << buf << endl;  
    }  
    
    ifs.close();  
}  
  
int main() {  
    test();  
    return 0;  
}
```

读取数据除了上述的，还有其他三种方式

```cpp
// 读取数据  
char buf1[1024] = {0};  
while ( ifs.getline(buf1, sizeof buf1) ) {  
	cout << buf1 << endl;  
}  

// 读取数据  
string buf2;  
while ( getline(ifs, buf2) ) {  
	cout << buf2 << endl;  
}  

// 读取数据  
char c;     // 效率低
while ((c = ifs.get()) != EOF ) {   // EOF end of file  
	cout << c << endl;  
}  

```


#### 二进制文件

##### 写文件

二进制方式写文件主要利用流对象调用成员函数 `write`

函数原型：`ostream& write( const char * buffer, int len );`

字符指针buffer指向内存中一段存储空间，len时读写的字节数

```cpp
class Person {  
public:  
    char name[64];  
    int age;  
};  
  
void test() {  
    ofstream ofs;  
    ofs.open(R"(F:\vs_code\c++\test.txt)", ios::in | ios::binary);  
    Person jack = {"jack", 18};  // 初始化列表   共有属性可以  
    ofs.write((const char *)&jack, sizeof(Person));    // (const char *)  强转性
    ofs.close();  
}
```

##### 读文件

```cpp
class Person {  
public:  
    char name[64];  
    int age;  
};  
  
void test() {  
    ifstream ifs;  
    ifs.open(R"(F:\vs_code\c++\test.txt)", ios::out | ios::binary);  
    if ( !ifs.is_open() ) {  
        return;  
    }  
    Person p;  
    ifs.read( (char *)&p,sizeof(Person) );  
    cout << p.name << endl;  
    cout << p.age << endl;  
    ifs.close();  
}
```


## C++ 提高

### 模板

#### 概念

模板就是建立通用的磨具，提高复用性

模板不能直接使用，只是提供了一个框架


#### 函数模板

C++除了链式编程的编程思想，还有泛型编程，主要利用的技术就是模板

##### 语法

函数模板作用
建立一个通用函数，其函数返回值类型和形参类型可以不具体定制，用一个虚拟的类型来表示

```cpp
template<type T>
函数声明或定义
```

template  ——  声明创建模板
typename —— 表明后面符号是一种数据类型，可以用class代替
T —— 通用数据类型

```cpp
// 声明模板函数  
template<typename T>  
void func(T &a, T &b) {  
    T temp = a;  
    a = b;  
    b = temp;  
    cout << a << endl;  
    cout << b << endl;  
}  
  
// 使用模板函数  
void test() {  
    double a, b;  
    a = 10.0;  
    b = 20.9;  
    // 自动类型推导
    func(a, b);  
    // 显式指定类型  
    func<double>(a, b);  
}
```

通用的数据类型可以存在多个，返回值、参数，都可以用通用数据类型替代

* 普通函数调用时可以发生隐式类型转换
* 函数模板再显示指定类型调用时可以发生隐式类型转换
* 函数模板在自动类型推导时不可以发生隐式类型转换


##### 调用规则

调用规则
* 如果都可以实现，优先调用普通函数
* 可以通过空函数模板强制调用函数模板
* 函数模板可以重载
* 如果函数模板可以产生更好的匹配优先调用函数模板

```cpp
func<>(a, b);   // 空模板强制调用  
```

#### 类模板

##### 语法

```cpp
// 声明类模板  
template<class T, class U, class C, class idType = int>  
class Person {  
public:  
    T name;  
    U age;  
    C sex;  
    idType id;  
};  
  
int main() {  
    Person<string, int, string> person;    // 默认值选填
    person.name = "jack";   
    person.age = 18;  
    person.sex = "男";  
    person.id = 10;  
    return 0;  
}
```

类模板没有自动推到类型的使用，在模板参数列表中可以有默认参数

##### 类模板成员函数创建时机

不同于普通类中成员函数的创建，类模板中创元函数在调用时才创建

```cpp
class Person_1 {  
public:  
    void func_1() {  
        cout << "Person_1" << endl;  
    }  
};  
  
class Person_2 {  
public:  
    void func_2() {  
        cout << "Person_2" << endl;  
    }  
};  
  
template<class T>  
class Person {  
public:  
    T obj;  
    void func_01() {  
        obj.func_1();  
    }  
    void func_02() {  
        obj.func_2();  
    }  
};
```

上述代码是可以通过编译，只有在真正调用模板类中的成员函数时，才会创建成员函数，然后确定是否可以运行


##### 类模板对象做函数参数

* 指定传入的我类型   ——   直接显示对象的数据类型
* 参数模板化              ——   将对象中的参数变为模板进行传递
* 整个类模板化          ——   将这个对象类型模板化进行传递


```cpp
template<class T, class U>  
class Person {  
public:  
    Person(T name, U age) {  
        this->name = name;  
        this->age = age;  
    }  
    T name;  
    U age;  
    void show() {  
        cout << this->name << endl;  
        cout << this->age << endl;  
    }  
};  
  
// 直接指定传入类型  
void showPerson(Person<string, int> &p) {  
    p.show();  
}  
  
// 参数模板化  
template<class T, class U>  
void showPerson_1(Person<T, U> &p) {  
    p.show();  
}  
  
// 整个类模板化  
template<typename T>  
void showPerson_2( T &p) {  
    p.show();  
}  
  
void test() {  
    Person<string, int> person("jack", 18);  
    showPerson(person);  
    showPerson_1(person);  
    showPerson_2(person);  
}  
  
int main() {  
    test();  
    return 0;  
}
```


##### 类模板与继承

当子类继承的父类是类模板时，子类在声明时需要指出父类中类型。如果不指定，编译器无法为子类分配内存。如果项灵活指出父类中的类型，子类也要变为类模板

```cpp
template<class T>  
class Person {  
public:  
    T t;  
};  

// 直接指定
class son : public Person<int> {  
      
};

// 在子类也是类模板
template<class U>
class son_! : public Person<U> {

};
```


##### 成员函数类外实现

```cpp
template<class T>  
class Person {  
public:  
    Person(T t);  
    T t;  
};  
  
template<class T>  
Person<T>::Person(T t) {  
    cout << "Person 构造函数" << endl;  
}
```

##### 类模板分文件编写

[类模板中成员函数的创建时机](#类模板成员函数创建时机)导致分文件编写链接不到

解决方案：
* 直接包含`.cpp` 源文件
* 将声明和实现写在同一个文件中，并更改后缀名为 `.hpp`

```cpp
// hpp/Person.h
#pragma once  
#include<iostream>  
  
  
template <class tName, class tAge>  
class Person {  
public:  
    Person(tName name, tAge age);  
    tName name;  
    tAge age;  
    void showPerson();  
};

// cpp/Person.cpp
#include "http://www.droliz.cn/hpp/Person.h"  
using namespace std;  
  
template<class tName, class tAge>  
Person<tName, tAge>::Person(tName name, tAge age) {  
    this->name = name;  
    this->age = age;  
}  
  
template<class tName, class tAge>  
void Person<tName, tAge>::showPerson() {  
    cout << this->name << endl;  
    cout << this->age << endl;  
}
```

主程序
```cpp
// main.cpp
#include <iostream>  
using namespace std;  
#include "./hpp/Person.hpp"  
  
int main() {  
    Person<string, int> jack("jack", 18);  
    jack.showPerson();  
    return 0;  
}
```

由于[类模板成员函数创建时机](#类模板成员函数创建时机)，在包含头文件时，编译器只看到声明，不会寻找或生成成员函数，导致无法链接到类模板中的成员函数

解决方案：
* 直接包含cpp源文件（不建议）
	```cpp
	#include <iostream>  
	using namespace std;  
	#include "./cpp/Person.cpp"  
	  
	int main() {  
	    Person<string, int> jack("jack", 18);  
	    jack.showPerson();  
	    return 0;  
	}
	```
* 将头文件和源文件写一起，后缀改为`.hpp`（约定为hpp，可以自行更改）
	```cpp
	// /hpp/Person.hpp
	#pragma once  
	#include<iostream>  
	using namespace std;  
	  
	template <class tName, class tAge>  
	class Person {  
	public:  
	    Person(tName name, tAge age);  
	    tName name;  
	    tAge age;  
	    void showPerson();  
	};  
	  
	template<class tName, class tAge>  
	Person<tName, tAge>::Person(tName name, tAge age) {  
	    this->name = name;  
	    this->age = age;  
	}  
	  
	template<class tName, class tAge>  
	void Person<tName, tAge>::showPerson() {  
	    cout << this->name << endl;  
	    cout << this->age << endl;  
	}
	
	// main.cpp
	#include <iostream>  
	using namespace std;    
	#include "./hpp/Person.hpp"  
	  
	int main() {  
	    Person<string, int> jack("jack", 18);  
	    jack.showPerson();  
	    return 0;  
	}
	```

##### 类模板与友元

* 全局函数类内实现
	* 直接在类内声明友元函数即可
	```cpp
	#include <iostream>  
	using namespace std;  
	  
	template<class T, class U>  
	class Animal {  
	public:  
	    friend void Print(Animal<T, U> animal) {    // 在类内实现
	        cout << animal.name << endl;  
	        cout << animal.age << endl;  
	    };  
	    Animal(T name, U age);  
	    T name;  
	    U age;  
	};  
	  
	template<class T, class U>  
	Animal<T, U>::Animal(T name, U age) {  
	    this->name = name;  
	    this->age = age;  
	}  
	  
	int main() {  
	    Animal<string, int> animal("cat", 18);  
	    Print(animal);    // 调用全局函数
	    return 0;  
	}
	
	```
* 全局函数内外实现
	* 提前声明全局函数的存在
	```cpp
	#include <iostream>  
	using namespace std;  
	template<class T, class U>  
	class Animal;   // 提前声明此类
	  
	// 类外实现  提前告知全局函数
	template<class T, class U>  
	void Print(Animal<T, U> animal) {  
	    cout << animal.name << endl;  
	    cout << animal.age << endl;  
	}  
	  
	template<class T, class U>  
	class Animal {  
	public:  
		// 空模板
	    friend void Print<>(Animal<T, U> animal);  
	    Animal(T name, U age);  
	    T name;  
	    U age;  
	};  
	  
	template<class T, class U>  
	Animal<T, U>::Animal(T name, U age) {  
	    this->name = name;  
	    this->age = age;  
	}  
	  
	int main() {  
	    Animal<string, int> animal("cat", 18);  
	    Print(animal);  
	    return 0;  
	}
	```


### STL

#### 初识 STL

C++的面向对象和泛型编程思想，目的就是复用性的提升。大多数情况下，数据结构和算法都未能有一套标准，导致被迫从事大量重复工作。为了建立数据结构和算法的一套标准，诞生了 STL

##### STL 基本概念

STL：Standard Template Libray，==标准模板库==
* 从广义上分为：**容器**、**算法**、**迭代器**
	* 容器和算法之间通过迭代器进行无缝链接
	* STL 几乎所有的代码都采用了模板类或者模板函数


##### STL 六大组件

* 容器
	* 各种数据结构，如 vector、list、deque、set、map等用来存放数据
* 算法
	* 各种常用的算法，sort、find、copy、for_each等
* 迭代器
	* 扮演容器和算法站之间的胶合剂
* 房函数
	* 行为类似函数，可作为算法的某种策略
* 适配器（配接器）
	* 一种用来修饰容器或者仿函数或迭代器接口
* 空间配置器
	* 负责空间的配置与管理


##### STL中容器、算法、迭代器

容器
* STL 容器就是将运用==最广泛的一些数据结构实现出来==
* 数组、链表、树、栈、队列、集合、映射表等
* 这些容器分为==序列式容器==和==关联式容器==
	* 序列式容器：强调值得排序，序列式容器中的每个元素均有固定的位置
	* 关联式容器：二叉树结构，各元素之间没有严格的物理上的顺序关系

算法
* 用有限的步骤，解决逻辑或数学上的问题
* 算法分为==质变算法==和==非质变算法==
	* 质变算法：是指运算过程中会更改区间内的元素内容，例如拷贝、替换、删除等
	* 非质变算法：是指运算过程中不会更改区间内的元素内容，例如查找、技术、遍历、寻找极值等

迭代器
* 提供一种方法，使之能够依序寻访某个容器所含的各个元素，而又无需暴露该容器的内部表达方式
* 每个容器都有专属的迭代器
	* 迭代器使用非常类似于指针

迭代去种类
|种类|功能|支持运算|
|:--|:--|:--|
|输入迭代器|对数据的只读访问|只读，支持++、\=\=、!=|
|输出迭代器|对数据的只写访问|只写，支持++|
|前向迭代器|读写操作，并能向前推进迭代器|读写，支持++、\=\=、!=|
|双向迭代器|读写操作，并能向前和向后操作|读写，++、--|
|随机访问迭代器|读写操作，可以以跳跃的方式访问任意数据|读写，支持++、--、\[n\]、-n、<、<=、>、>=|

常用的迭代器中，迭代器为双向迭代器或随机访问迭代器

##### 容器算法迭代器

STL 中最常用的容器为 Vector，可以理解为数组

**vector存放内置数据类型**

容器：`vector`
算法：`for_each`
迭代器：`vector<int>::iterator`

```cpp
#include <iostream>  
using namespace std;  
#include "vector"     // 包含头文件  
#include "algorithm"  // 标准算法头文件  

// 回调函数
void callback(int val) {  
    cout << val << endl;  
}  
  
  
void test(vector<int> v) {  
    // 插入数据  
    for (int i = 10; i <= 30; i += 10) {  
        v.push_back(i);  
    }  
    // 通过迭代器访问  
    vector<int>::iterator itBegin = v.begin();  // 起始迭代器，指向容器第一个元素位置  
    auto itEnd = v.end();  // 指向容器中最后一个元素的下一个位置  
	  
    // 遍历方式 1  
    while ( itBegin != itEnd ) {  
        cout << *itBegin << endl;  
        itBegin++;  
    }  
	  
    // 遍历   2
    for (auto it = v.begin(); it != v.end(); it++) {  
        cout << *it << endl;  
    }  
	  
    // 使用 STL 提供的遍历算法  
    for_each(v.begin(), v.end(), callback);   // 起始，结束，回调  
}  
  
  
int main() {  
    vector<int> v;  
    test(v);  
    return 0;  
}
```

[auto存储类](#auto存储类)简化，但在c++19被废弃

遍历方法二时源于方法一的简化，但方法二还可以更加简化
```cpp
for (int & it : v) {    // c++允许简单的范围迭代
    cout << it << endl;  
}
```

##### 存放自定义数据类型


存放数据类型
```cpp
#include <iostream>  
using namespace std;  
#include "vector"     // 包含头文件  
  
class Animal {  
public:  
    Animal(string name, int age);  
    string name;  
    int age;  
};  
  
Animal::Animal(string name, int age) {  
    this->name = move(name);  
    this->age = age;  
}  
  
  
void test(vector<Animal> v) {  
    Animal a1("a1", 19);  
    Animal a2("a2", 19);  
    Animal a3("a3", 20);  
  
    v.push_back(a1);  
    v.push_back(a2);  
    v.push_back(a3);  
  
	for (Animal &a : v) {  
	    cout << a.name << " | ";  
	    cout << a.age << endl;  
	}  
	
	// it 类似指针，指向保存的数据的地址。这里相当于   * it = Animal
	for ( vector<Animal>::iterator it = v.begin(); it != v.end(); it++ ) {  
	    cout << it->name << " | ";  
	    cout << it->age << endl;  
	}
  
}  
  
  
int main() {  
    vector<Animal> v;  
    test(v);  
    return 0;  
}
```


如果上述存放的是指向数据类型的指针
```cpp
#include <iostream>  
using namespace std;  
#include "vector"     // 包含头文件  
  
class Animal {  
public:  
    Animal(string name, int age);  
    string name;  
    int age;  
};  
  
Animal::Animal(string name, int age) {  
    this->name = move(name);  
    this->age = age;  
}  
  
  
void test(vector<Animal *> v) {  
    Animal a1("a1", 19);  
    Animal a2("a2", 19);  
    Animal a3("a3", 20);  
  
    v.push_back(&a1);  
    v.push_back(&a2);  
    v.push_back(&a3);  
	
	// 这里存放指针，相当于   * it = & Animal 
    for ( vector<Animal *>::iterator it = v.begin(); it != v.end(); it++ ) {  
        cout << (*it)->name << " | ";  
        cout << (*it)->age << endl;  
    }  
  
}  
  
  
int main() {  
    vector<Animal *> v;  
    test(v);  
    return 0;  
}
```


在解引用时，`*it = value` 就是数组中的值，将 `*it` 看为整体即可

##### 容器嵌套容器

```cpp
void test() {  
    vector<vector<int>> v;  
  
    vector<int> v1, v2, v3;  
  
    for ( int i = 0; i < 4; i++ ) {  
        v1.push_back(i);  
        v2.push_back(i + 2);  
        v3.push_back(i + 3);  
    }  
  
    v.push_back(v1);  
    v.push_back(v2);  
    v.push_back(v3);  
  
    for ( vector<vector<int>>::iterator row = v.begin(); row < v.end(); row++ ) {  
        for ( vector<int>::iterator col = (*row).begin(); col < (*row).end(); col ++ ) {  
            cout << *col << "\t";  
        }  
        cout << "\n-----------------" << endl;  
    }  
}
```

#### 常用容器

##### string容器

头文件：`#include<string>`

**基本概念**

string本质上是一个类

string与char * 区别
* char * 是一个指针
* string 是一个类，内部封装了 char \*，管理这个字符串，是一个char \* 型的容器

string 类封装了很多成员方法：find、copy、delete、relate、insert等，string管理 char\*所分配的内存，复制越界和取值月结等由类内部负责

**构造函数**

构造函数原型
* `string();` ：创建空字符串
* `string(const char* s);`：使用字符串 s 初始化
* `string(const string& str);`：使用 string 对象初始化
* `string(int n, char c);`：使用 n 个字符 c 初始化

```cpp
void test() {  
    string s = string();  
    const char * a = "aaa";  
    string s1 = string(a);   // string("aaa");
    string s2 = string(s1);  
    string s3 = string(3, 'a');  
}
```

**赋值操作**

赋值函数原型
* `string& operator = (const char* s);`
* `string& operator = (constr string &s);`
* `string& operator = (char c);`
* `string& asign(const char *s);`
* `string& asign(const char *s, int n);`：将字符串s的前n个字符赋值给当前字符串
* `string& asign(const string &s);`
* `string& asign(int n, char c);`：用n个字符c赋值给当前字符串

```cpp
void test() {  
    string s = string();  
    const char * chr = "aaa";  
    string s1("bbb");  
  
    s = chr;  
    s = s1;  
    s = 'a';  
    s.assign(chr);  
    s.assign(chr, 2);  
    s.assign(s1);  
    s.assign(4, 'c');  
}
```

**字符串拼接**

赋值函数原型
* `string& operator += (const char* s);`
* `string& operator += (constr string &s);`
* `string& operator += (char c);`
* `string& append(const char *s);`
* `string& append(const char *s, int n);`：将字符串s的前n个字符拼接到字符串尾部
* `string& append(const string &s);`
* `string& asign(const string &s, int pos, char n);`：s从pos开始的n个字符拼接

**字符串查找和替换**

查找： `find()` 和 `rfind()`  查找返回的是索引

替换：`replace()`  替换会返回替换后的新字符串

**字符串比较**

字符串之间按字符的 ASCII 码比较

比较：`compare` 
* = 返回  0
* > 返回  1
* < 返回  -1

**字符串存取**

string中单个字符存取方式有两种
* `char& operator[](int n);`   通过 [ ] 方式取字符
* `char& at(int n)`   通过 at 获取

**字符串插入和删除**

插入：`insert();`
删除：`erase();`


**子串获取**

`substr(int pos = 0, int n = npos)`  返回由 pos 开始的 n 个字符组成的字符串

##### vector容器

头文件：`#include<vector>`

**基本概念**

vector 数据结构和数组非常相似，也成为==单端数组==

相较于普通数组的静态空间，vectoer是可以动态扩展的

动态扩展：找更大的内存空间，将原数据拷贝到新空间，释放原空间

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220816140656.png)

**构造函数**

* `vector<T> v;`  
* `vector<v.begin(), v.end()>;`   v 这个区间中的元素拷贝过来
* `vector<n, elem>;`    n 个 elem 拷贝给自身
* `vector<const vector &vec>;`    拷贝构造函数

```cpp
void test() {  
    vector<int> v;  
    vector<int> v1(v.begin(), v.end());  
    vector<int> v2(2, 3);
    vector<int> v3((const vector<int>)v2);  
}
```

**赋值**

* `v = ( const vector &vec );`
* `v.assign(beg, end);`
* `v.assign(n, elem)`

```cpp
void print(vector<int> &v) {  
    for (int val : v) {  
        cout << val << " ";  
    }  
    cout << endl;}  
  
void test() {  
    vector<int> v(7, 2);  
    vector<int> v1;  
  
    v1 = (const vector<int>)v;  
    print(v1);  
  
    v1.assign(4, 3);  
    print(v1);  
  
    v1.assign(v.begin(), v.end());  
    print(v1);  
}
```


**容量大小**

* `empty();`：判断是否为空
* `capacity();`：容器容量
* `size();`：元素个数（一定小于等于容量）
* `resize(int num, elem);`：重新指定长度，若变长，以elem填充（没写则以默认值填充），若变短，删除超出部分


**插入删除**

* `push_back(elem);`：尾部插入
* `pop_back();`：删除最后一个元素
* `insert(const_iterator pos, int count, elem);`：向指定位置 pos 添加 count（选填）个元素 elem
* `erase(const_iterator start, const_iterator end);`：删除区间元素，如果只填一个，删除指定位置元素
* `clear();`：清空


**数据存取**

* `at(int pos);`：返回索引位置元素
* `v[pos];`：返回索引未知元素
* `front();`：返回第一个元素
* `back();`：返回最后一个元素


**互换容器**

* `swap(vec);`：将 vec 与本身元素互换，只更换值

```cpp
void test() {  
    vector<int> v(7, 2);  
    vector<int> v1(7, 3);  
    cout << &v << " | " << &v1 << endl;  
    v.swap(v1);  
    cout << &v << " | " << &v1 << endl;  
}
```

**预留空间**

减少 vector 在动态扩展容量时的扩展次数

* `reserve(int len);`  预留 len 个元素长度，预留位置不可初始化，元素不可访问

```cpp
void test() {  
    vector<int> v;  
  
    int count = 0;  // 统计开辟空间次数  
    int *p = nullptr;  
	// v.reserve(10000);   // 如果没有预留长度，那么会不断开辟内存空间
    for (int i = 0; i < 10000; i++) {  
        v.push_back(i);  
  
        if (p != &v[0]) {  
            p = &v[0];  
            count++;  
        }  
    }  
    cout << count << endl;  
}


// 15

// 预留长度为1000，count = 1
```

##### deque容器

头文件：`#include<deque>`

**基本概念**

双端数组，可以堆头端进行插入删除操作

vector 对于头部插入删除效率低，但 vector 范文元素速度比 deque 快

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220816145332.png)


deque 内部有个==中控器==，维护每段缓冲区的内容，缓冲区中存放真实数据，中控器维护的是每个缓冲区的地址，是的使用deque时向一片连续的内存空间

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220816150101.png)

如果要头插入，那么就在 `0x01` 缓冲区前面添加 `ele`，反之在 `0x03` 插入，当一个地址指向的缓冲区满了，就会开辟新的缓冲区，然后将地址保存在中控器上

**构造函数**

* `deque<T> deq;`
* `deque(beg, end);`：拷贝区间 \[ beg, end ) 的元素个自身
* `deque(n, elem);`
* `deque(const deque *deq);`

**赋值操作**

* `deq = (const deque &deq);`
* `assign(begin, end);`
* `assign(n, elem);`

**大小操作**

与 vector 相同

* `empty();`
* `size();`
* `resize(int num, elem);` 

**插入删除**

两端插入删除
* `push_back(elem);`
* `push_front(elem);`
* `pop_back();`
* `pop_front();`

指定位置操作
* `insert(pos, n, elem);`  n 选填，如果没填 n ，返回新数据位置
* `insert(pos, begin, end);` 插入一段数据
* `clear();`
* `erase(begin, end);`：返回下一个数据的位置
* `erase(pos);`：返回下一个数据位置

**数据存取**

* `at(int pos);`
* `deque[pos];`
* `front();`
* `back();`

**排序操作**

头文件：`#include<algorithm>`  标准算法库

* `sort(iterator begin, oterator end);` 堆区间\[ begin,  end) 排序

##### stack容器

头文件：`#include<stack>`

**概念**

stack （栈）是一种==先进后出==的数据结构，只有一个出口

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220816155847.png)

栈中只有栈顶元素可以被外界访问，故栈式不允许由遍历行为的

**构造函数**

* `stack<T> stack;`
* `stack(const stack &stack);`

**赋值操作**

* `stack = ( const stack &stk );`

**数据存取**

* `push(elem);`    入栈
* `popp();`   出栈
* `top();`    获取栈顶元素 

**大小操作**

* `empty();`
* `size();`


##### queue容器

头文件：`#inlude<queue>`

**基本概念**

Queue 是一种==先进先出==的数据结构，由两个出口

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220816161303.png)


只有队头和队尾可以被外界访问，不允许访问中间值

**构造函数**

* `queue<T> que;`
* `queue(const queue &que);`

**赋值操作**

* `que = (const queue & queue);`

**数据存取**

* `push(elem);`    入列
* `pop();`      出列
* `back();`     队头元素
* `front();`   队尾元素

**大小操作**

* `empty();`
* `size();`

##### list容器

头文件：`#include<list>`

**基本概念**

将数据进行链式存储

list（链表）式一种==物理存储单元上非连续==的存储结构，数据元素的逻辑顺序式通过链表中的指针链接实现的

链表是由一系列节点组成的，节点由存储数据元素的数据域和指向下一个节点的指针组成

STL 中的链表是一个双向孙环链表


![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220816170824.png)

链表的存储方式不是连续的内存空间，仅支持前移或后移


**构造函数**

* `list<T> li;`
* `list(begin, end);`
* `list(int n, elem);`
* `list(const list &li);`

**赋值和交换**

* `assign(begin, end);`
* `assign(int n, elem);`
* `li = ( const list &li );`
* `swap(li);`

**大小操作**

* `size();`
* `empty();`
* `resize(num, elem);`

**插入删除**

* `push_back(elem);`
* `pop_back();`
* `push_front(elem);`
* `pop_front();`
* `insert(int pos, n, elem);`    如果没 n， 返回新数据位置
* `insert(pos, begin, end);`
* `clear();`
* `erase(begin, end);`     返回下一个数据位置
* `erase(pos);`      返回下一个数据位置
* `remove(elem);`   删除所有与 elem 值匹配的元素

**数据存去**

* `front();`
* `back();`


##### set/multiset容器

**基本概念**

* set
	* 所有元素都会在插入时自动排序
* set / multiset 属于关联式容器，底层结构使用==二叉树==实现
* 相比 set ，multiset允许由重复的元素，而set不允许


**构造函数和赋值**

构造：
* `set<T> st;`
* `set(const set &st);`

赋值：
* `st = (const set &st);`


**大小和交换**

* `size();`
* `empty();`
* `swap(st);`

**插入和删除**

* `insert(elem);`
* `clear();`
* `erase(pos);`   // 返回下一个元素迭代器
* `erase(begin, end);`   返回下一个元素迭代器
* `erase(elem);`

**查找统计**

* `find(key);`     如果存在，返回该键元素的迭代器；负责返回 `st.end()`
* `count(key);` 

**区别**

set插入数据同时会返回插入结构，表示插入是否成功，而multiset不会检测数据

**set容器排序**

利用[仿函数](#函数对象)，可以改变排序规则

set存放内置数据类型
```cpp
class Sort {  
public:  
    bool operator()(int v1, int v2) const {  
        return v1 > v2;  
    }  
};  
  
void test() {  
    // 指定排序规则为从大到小  
    set<int, Sort> s;  
  
    for (int i = 10; i < 80; i += 10) {  
        s.insert(i);  
    }  
  
    for (int i: s) {  
        cout << i << endl;  
    }  
};
```

set存放自定义数据类型
```cpp
#include <iostream>  
using namespace std;  
#include "set"  
  
class Animal {  
public:  
    Animal(string name, int age);  
    string name;  
    int age;  
  
};  
  
Animal::Animal(string name, int age) {  
    this->name = move(name);  
    this->age= age;  
}  
  
class Sort {  
public:  
    // 按年龄排序  
    bool operator()(const Animal &v1, const Animal &v2) const {  
        return v1.age > v2.age;  
    }  
};  
  
  
void test() {  
    set<Animal, Sort> s;
  
    Animal a1("a", 1);  
    Animal a2("b", 2);  
    Animal a3("c", 3);  
  
    s.insert(a1);  
    s.insert(a2);  
    s.insert(a3);  
  
    for (set<Animal, Sort>::iterator it = s.begin(); it != s.end(); it ++) {  
        cout << (*it).age << endl;  
    }  
}  
  
int main() {  
  
    test();  
    return 0;  
}
```

##### map/multimap容器

头文件：`#include<map>`

**基本概念**

map中的所有元素都是[pair](#命名空间std)，第一个元素为 kay，第二个元素为 value，所有元素会根据 key 自动排序

map与multimap的关系和set与multiset关系一样

**构造函数和赋值**

构造
* `map<T1, T2> mp;`
* `map(const map &mp);`

赋值
* `mp = ( const map &mp);`

**大小和交换**

* `size();`
* `empty();`
* `swap(mp);`

**插入和删除**

* `insert(elem);`
* `clear();`
* `erase(pos);`    删除 pos 迭代器所指元素，返回下一个元素迭代器
* `erase(begin, end);`  返回下一个元素迭代器
* `erase(key);` 

**查找统计**

* `find(key);`  返回该元素的迭代器，或mp.end()
* `count(key);`

```cpp
void test() {  
    map<int, int> mp;  
    mp.insert(pair<int, int>(1, 10));  
    mp.insert(pair<int, int>(2, 20));  
    mp.insert(pair<int, int>(3, 30));  
  
    map<int, int>::iterator pos = mp.find(3);  
  
    cout << pos->first << endl;  
    cout << pos->second << endl;  
}
```

**排序**

利用仿函数更改排序规则

内置数据类型
```cpp
class Sort {  
public:  
    bool operator()(int v1, int v2) {  
        return v1 > v2;  
    }  
};  
  
void test() {  
    map<int, int, Sort> mp;  
    mp.insert(make_pair(1, 10));  
    mp.insert(make_pair(2, 20));  
    mp.insert(make_pair(3, 30));  
  
    for (pair<int, int> p: mp) {  
        cout << p.first << " | ";  
        cout << p.second << endl;  
    }  
}
```


自定义数据类型
```cpp
class Sort {  
public:  
    bool operator()(const int v1, const int v2) const {  
        return v1 > v2;  
    }  
};  
  
void test() {  
    Animal a1(10, "a1");  
    Animal a2(20, "a2");  
    Animal a3(30, "a3");  
  
    map<int, string, Sort> mp;  
    mp.insert(make_pair(a1.id, a1.name));  
    mp.insert(make_pair(a2.id, a2.name));  
    mp.insert(make_pair(a3.id, a3.name));  
  
    for (pair<int, string> p: mp) {  
        cout << p.first << " | ";  
        cout << p.second << endl;  
    }  
}
```


#### 函数对象

##### 概念

重载**函数调用操作符**的类，其对象成为函数对象，函数对象在使用重载的 `()` 时，行为类似函数的调用，也称为仿函数。仿函数是一个类，不是函数

##### 函数对象的使用

* 函数对象在使用时，可以向普通函数一样调用，可以有参数，返回值
* 函数对象超出普通函数的概念，函数对象可以有自己的状态
* 函数对象可以作为参数传递


```cpp
// 仿函数  
class Add {  
public:  
    int operator()(int v1, int v2) {  
        count++;  
        return v1 + v2;  
    }  
    Add(): count(0) {};  
    int count;   // 记录调用次数，获取调用状态  
};  
  
void test() {  
    Add add_01;  
    // 像普通函数调用  
    const int c = add_01(10, 10);  
    cout << c << endl;  
    // 作为参数传递  
    cout << add_01(add_01(0, 1), 1) << endl;  
}
```

##### 谓词

返回值为 `bool` 类型的仿函数称为谓词，如果接收参数为一个则为一元谓词，两个参数则为二元谓词

谓词是一个可调用的表达式，其返回结果是一个能用作条件的值

谓词可作为一个判断式

**一元谓词**

```cpp
#include <iostream>  
using namespace std;  
#include "algorithm"  
#include "vector"  
  
// 结构体  
struct GreaterFive {  
    bool operator()(int val);  
};  
  
bool GreaterFive::operator()(int val) {  
    return val % 2;  
}  
  
void test() {  
    vector<int> v;  
    for (int i = 1; i <= 10; i++) {  
        v.push_back(i);  
    }  
	// find_if返回迭代器，第三个参数是对象
    vector<int>::iterator it = find_if(v.begin(), v.end(), GreaterFive());  
	  
    cout << *it << endl;  // 1
}  
  
int main() {  
  
    test();  
    return 0;  
}
```

**二元谓词**

```cpp
#include <iostream>  
using namespace std;  
#include "algorithm"  
#include "vector"  
  
// 结构体  
struct GreaterFive {  
    bool operator()(int v1, int v2);  
};  
  
bool GreaterFive::operator()(int v1, int v2) {  
    return v1 > v2;  
}  
  
void test() {  
    vector<int> v;  
    for (int i = 1; i <= 10; i++) {  
        v.push_back(i);   
    }  
  
    sort(v.begin(), v.end(), GreaterFive());  
  
    for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {  
        cout << (*it) << endl;  
    }  
}  
  
int main() {  
  
    test();  
    return 0;  
}
```


##### 内建函数对象

STL 内建了一些函数对象
* 算术仿函数
* 关系仿函数
* 逻辑仿函数

这些仿函数所产生的对象，用法和一般函数完全相同，使用这些内建函数对象，需要引用头文件`#include<functional>`

**算术仿函数**

除了 nefate 式一元运算，其他都是二元运算

* `class plus`  加法
* `class minus`  减法
* `class multies`  乘法
* `class divides`  除法
* `class modulus`   取模
* `class negate`  取反（符号取反）

```cpp
#include <iostream>  
using namespace std;  
#include "functional"  
  
void test() {  
    plus<int>p;  
    cout << p(10, 20) << endl;  
    negate<int>n;  
    cout << n(50) << endl;  
}  
  
int main() {  
    test();  
    return 0;  
}
```

**关系仿函数**

* `class equal_to`  等于
* `class not_equal_to`  不等于
* `class greate`  大于
* `class greate_equal`  大于等于
* `class less`   小于
* `class less_equal`  小于等于

**逻辑仿函数**

* `class logical_and`  与
* `class logical_or`    或
* `class logicla_not`  非


#### 常用算法

算法主要有头文件：`<algorithm>、<functional>、<numeric>`组成

##### 遍历

* **for_each**
	* `for_each(iterator begin, iterator end, _func)`    \_func：函数或仿函数
	```cpp
	void print(int v) {  
	    cout << v << endl;  
	}  
	  
	void test() {  
	    vector<int> v;  
	    for (int i = 0; i < 10; i += 2) {  
	        v.push_back(i);  
	    }  
	    for_each(v.begin(), v.end(), print);  // 普通函数放函数名，仿函数放函数对象
	    
	    // lambda 表达式
	    for_each(v.begin(), v.end(), [](int val){cout << val << endl;})
	}
	```
* **transform**
	* `transform(iterator beg1, iterator end1, iterator beg2, _func)`
	```cpp
	void test() {  
	    vector<int> v;  
	    for (int i = 0; i < 10; i += 2) {  
	        v.push_back(i);  
	    }  
	    vector<int> v1;  
	    v1.resize(10);  
	  
	    transform(v.begin(), v.end(), v1.begin(), [](int val) -> int {  
	        return val;  
	    });  
	  
	    for_each(v1.begin(), v1.end(), [](int val) -> void {  
	        cout << val << endl;  
	    });  
	}
	```


##### 查找

* `find`
* `find_if`
* `adjacont_find`     查找相邻元素
* `binary_search`     二分查找
* `count`
* `count_if`

##### 排序

* `sort`
* `random_shuffle`   随机调整
* `merge`     容器元素合并
* `reverse`   反转

##### 拷贝和替换

* `copy`
* `replace`
* `replace_if`
* `swap` 


## 其他

### Lambda 表达式

#### 概述

匿名函数（英文名：[lambda](https://so.csdn.net/so/search?q=lambda&spm=1001.2101.3001.7020)）就是没有名字的函数。最简单的匿名函数是[](){}，它没有参数也没有返回值。在匿名函数中，[]里面用来捕获函数外部的变量，而()里面就是匿名函数的参数，{}里面就是函数的执行代码。

> 匿名函数，也成lambda函数或lambda[表达式](https://so.csdn.net/so/search?q=%E8%A1%A8%E8%BE%BE%E5%BC%8F&spm=1001.2101.3001.7020)；

**基础示例**

```cpp
#include <iostream>  
  
using namespace std;  
  
int main()  
{  
    auto func = [] () { cout << "Hello world"; };  
    func(); // now call the function  
}
```

其中func就是一个lambda函数。我们使用auto来自动获取func的类型，这个非常重要。定义好lambda函数之后，就可以当这场函数来使用了。

#### lambda表达式

Lambda表达式具体形式如下:

```cpp
[capture](parameters)->return-type{body}
```

如果没有参数,空的圆括号()可以省略.返回值也可以省略,如果函数体只由一条return语句组成或返回类型为void的话.形如:

```cpp
[capture](parameters){body}
```

下面举了几个Lambda函数的例子:

```cpp
[](int x, int y) { return x + y; } // 隐式返回类型 
[](int& x) { ++x; } // 没有return语句 -> lambda 函数的返回类型是'void' 
[]() { ++global_x; } // 没有参数,仅访问某个全局变量 
[]{ ++global_x; } // 与上一个相同,省略了()
```

可以像下面这样显示指定返回类型:

```cpp
[](int x, int y) -> int { int z = x + y; return z; }
```

在这个例子中创建了一个临时变量z来存储中间值. 和普通函数一样,这个中间值不会保存到下次调用. 什么也不返回的Lambda函数可以省略返回类型, 而不需要使用 -> void 形式.

##### Lambda函数中的变量截取

Lambda函数可以引用在它之外声明的变量. 这些变量的集合叫做一个闭包. 闭包被定义在Lambda表达式声明中的方括号[]内. 这个机制允许这些变量被按值或按引用捕获.下面这些例子就是:

```txt
[]			//不截取任何变量,试图在Lambda内使用任何外部变量都是错误的（全局变量除外）.
[&]			//截取外部作用域中所有变量，并作为引用在函数体中使用
[=] 		//截取外部作用域中所有变量，并拷贝一份在函数体中使用
[=, &foo]   //截取外部作用域中所有变量，并拷贝一份在函数体中使用，但是对foo变量使用引用
[bar]   	//截取bar变量并且拷贝一份在函数体重使用，同时不截取其他变量
[this]		//截取当前类中的this指针。如果已经使用了&或者=就默认添加此选项。
-----------------------------
[x, &y]  	//x 按值捕获, y 按引用捕获.
[&, x]		//x显式地按值捕获. 其它变量按引用捕获
[=, &z]		//z按引用捕获. 其它变量按值捕获
```

#### Lambda函数和STL

lambda函数的引入为STL的使用提供了极大的方便。比如下面这个例子，当你想便利一个vector的时候，原来你得这么写：

```cpp
vector<int> v;  
v.push_back( 1 );  
v.push_back( 2 );  
//...  
for ( auto itr = v.begin(), end = v.end(); itr != end; itr++ )  
{  
    cout << *itr;  
}  
```

现在有了lambda函数你就可以这么写

```cpp
vector<int> v;  
v.push_back( 1 );  
v.push_back( 2 );  
//...  
for_each( v.begin(), v.end(), [] (int val)  
{  
	cout << val;  
} );  
```

而且这么写了之后执行效率反而提高了。因为编译器有可能使用”循环展开“来加速执行过程（计算机系统结构课程中学的）。  
[http://www.nwcpp.org/images/stories/lambda.pdf](http://www.nwcpp.org/images/stories/lambda.pdf) 这个PPT详细介绍了如何使用lambda表达式和STL



### 异常处理

异常提供了一种转移程序控制权的方式。C++ 异常处理涉及到三个关键字：**try、catch、throw**。

-   **throw:** 当问题出现时，程序会抛出一个异常。这是通过使用 **throw** 关键字来完成的。
-   **catch:** 在您想要处理问题的地方，通过异常处理程序捕获异常。**catch** 关键字用于捕获异常。
-   **try:** **try** 块中的代码标识将被激活的特定异常。它后面通常跟着一个或多个 catch 块。

```cpp
#include <iostream>  
using namespace std;  
  
double division(int a, int b)  
{  
    if( b == 0 )  
    {  
        // 抛出异常  
        throw "Division by zero condition!";  
    }  
    return (a/b);  
}  
  
int main ()  
{  
    int x = 50;  
    int y = 0;  
    double z = 0;  
  
    try {  
        z = division(x, y);  
        cout << z << endl;  
    }catch (const char* msg) {   // 捕获异常  
        cerr << msg << endl;   // cerr：标准的错误输出流
    }  
  
    return 0;  
}
```

throw 语句的操作数可以是任意的表达式，表达式的结果的类型决定了抛出的异常的类型。



![C++标准异常](https://www.runoob.com/wp-content/uploads/2015/05/exceptions_in_cpp.png)

### 动态内存

在 C++ 中，可以使用特殊的运算符为给定类型的变量在运行时分配堆内的内存，这会返回所分配的空间地址。这种运算符即 **new** 运算符。

如果不再需要动态分配的内存空间，可以使用 **delete** 运算符，删除之前由 new 运算符分配的内存。

new运算符返回的是地址

#### 检点数据类型的动态内存

```cpp
#include <iostream>  
using namespace std;  
  
int main () {  
    double* pvalue  = NULL; // 初始化为 null 的指针  
    pvalue  = new double;   // 为变量请求内存  
	  
    *pvalue = 29494.99;     // 在分配的地址存储值  
    cout << "Value of pvalue : " << *pvalue << endl;  
	  
    delete pvalue;         // 释放内存  
	  
    return 0;  
}
```

#### 数组的动态内存

```cpp
// 一维数组
// 动态分配,数组长度为 
m int *array=new int [m]; 
//释放内存 
delete [] array;


// 二维数组
int **array  
// 假定数组第一维长度为 m， 第二维长度为 n// 动态分配空间  
array = new int *[m];  
for( int i=0; i<m; i++ ) {  
	array[i] = new int [n]  ;  
}  
//释放  
for( int i=0; i<m; i++ ) {  
	delete [] array[i];  
}  
delete [] array;
```

#### 对象的动态内存分配

```cpp
#include <iostream>  
using namespace std;  
  
class Box {  
public:  
    Box() {  
        cout << "调用构造函数！" << endl;  
    }  
    ~Box() {  
        cout << "调用析构函数！" << endl;  
    }  
};  
  
int main() {  
    Box* myBoxArray = new Box[4];  
  
    delete [] myBoxArray; // 删除数组  
    return 0;  
}

// 会先后调用 4 次构造和析构
```


### 命名空间

引入了**命名空间**这个概念，可作为附加信息来区分不同库中相同名称的函数、类、变量等。使用了命名空间即定义了上下文。本质上，命名空间就是定义了一个范围

例如：
![](https://www.runoob.com/wp-content/uploads/2019/09/0129A8E9-30FE-431D-8C48-399EA4841E9D.jpg)

#### 命名空间定义

```cpp
#include <iostream>  
using namespace std;  
  
// 第一个命名空间  
namespace first_space {  
    void func() {  
        cout << "Inside first_space" << endl;  
    }  
}  
// 第二个命名空间  
namespace second_space {  
    void func() {  
        cout << "Inside second_space" << endl;  
    }  
}  
  
int main() {  
    // 调用第一个命名空间中的函数  
    first_space::func();  
    // 调用第二个命名空间中的函数  
    second_space::func();  
    return 0;  
}

// Inside first_space
// Inside second_space
```


#### using指令

可以使用 **using namespace** 指令，这样在使用命名空间时就可以不用在前面加上命名空间的名称。这个指令会告诉编译器，后续的代码将使用指定的命名空间中的名称

```cpp
#include <iostream>  
using namespace std;  
  
// 第一个命名空间  
namespace first_space {  
    void func() {  
        cout << "Inside first_space" << endl;  
    }  
}  
// 第二个命名空间  
namespace second_space {  
    void func() {  
        cout << "Inside second_space" << endl;  
    }  
}  
  
using namespace first_space;  
int main() {  
    func();  
    return 0;  
}  
  
// Inside first_space
```

using 指令也可以用来指定命名空间中的特定项目。例如：如果只打算使用 std 命名空间中的 cout 部分，可以使用如下的语句：

```cpp
#include<iostream> 
using std::cout; 
int main () { 
	cout << "std::endl is used with std!" << std::endl; 
	return 0; 
}
```

#### 不连续命名空间

命名空间可以定义在几个不同的部分中，因此命名空间是由几个单独定义的部分组成的。一个命名空间的各个组成部分可以分散在多个文件中。

所以，如果命名空间中的某个组成部分需要请求定义在另一个文件中的名称，则仍然需要声明该名称。下面的命名空间定义可以是定义一个新的命名空间，也可以是为已有的命名空间增加新的元素

```cpp
namespace namespace_name { 
	// 代码声明 
}
```

#### 嵌套命名空间

命名空间可以嵌套，您可以在一个命名空间中定义另一个命名空间，如下所示：

```cpp
namespace namespace_name1 { 
	// 代码声明 
	namespace namespace_name2 { 
		// 代码声明 
	} 
}
```

```cpp
#include <iostream>  
using namespace std;  
  
namespace A {  
    int a = 100;  
    namespace B {          //嵌套一个命名空间B  
        int a = 20;  
    }  
}  
  
int a = 200; //定义一个全局变量  使用全局变量必须 ::a 否则报错
  
  
int main() {  
    cout << "A::a =" << A::a << endl;        //A::a =100  
    cout << "A::B::a =" << A::B::a << endl;  //A::B::a =20  
    cout << "a =" << a << endl;              //a =200  
    cout << "::a =" << ::a << endl;          //::a =200  
  
    using namespace A;  
    // cout << "a =" << a << endl;     // Reference to 'a' is ambiguous // 命名空间冲突，编译期错误  
    cout << "::a =" << ::a << endl; //::a =200  
  
    int a = 30;  
    cout << "a =" << a << endl;     //a =30  
    cout << "::a =" << ::a << endl; //::a =200  
  
    //即：全局变量 a 表达为 ::a，用于当有同名的局部变量时来区别两者。  
  
    using namespace A;  
    cout << "a =" << a << endl;     // a =30  // 当有本地同名变量后，优先使用本地，冲突解除  
    cout << "::a =" << ::a << endl; //::a =200  
    return 0;  
}


A::a =100
A::B::a =20
a =200
::a =200
::a =200
a =30
::a =200
a =30
::a =200
```


### 预处理器

预处理器是一些指令，指示编译器在实际编译之前所需完成的预处理。

所有的预处理器指令都是以井号（#）开头，只有空格字符可以出现在预处理指令之前。预处理指令不是 C++ 语句，所以它们不会以分号（;）结尾，预处理器指令只能在一行代码

#### # define 预处理

\#define 预处理指令用于创建符号常量。该符号常量通常称为**宏**，指令的一般形式是：

```cpp
#define DAY = 7
#define PI = 3.14

#define MIN(a,b) (a<b ? a : b)   // 带有参数的宏
```


#### 条件编译

有几个指令可以用来有选择地对部分程序源代码进行编译。这个过程被称为条件编译条件预处理器的结构与 if 选择结构很像

```cpp
#include <iostream>  
using namespace std;  
#define DEBUG    
  
#define MIN(a,b) (((a)<(b)) ? a : b)  
  
int main ()  
{  
    int i, j;  
    i = 100;  
    j = 30;  
#ifdef DEBUG  // DEBUG 表示只在调试时编译
    cerr <<"Trace: Inside main function" << endl;  
#endif  
  
#if 0  
    /* 这是注释部分 */   cout << MKSTR(HELLO C++) << endl;  
#endif  
  
    cout <<"The minimum is " << MIN(i, j) << endl;  
  
#ifdef DEBUG  
    cerr <<"Trace: Coming out of main function" << endl;  
#endif  
    return 0;  
}
```

```cpp
// 1
#if SOMETHING >= 100  
    //...  
#else  
//...  
#endif  

// 2
#ifndef SOMETHING_H  
#define SOMETHING_H  
//...  
#endif  

// 3
#if (defined DEBUG) && (defined SOMETHING)  
    //...  
#endif  

// 4
#ifdef SOMETHING  
    int func1(){/*...*/}  
#else  
    int func2() {/*...*/}  
#endif  

// 5
#ifdef SOMETHING  
    namespace space1{  
#endif  
//...  
#ifdef SOMETHING  
    }//space1  
#endif
```

#### # 和 ## 以及 \\ 运算符

\# 和 ## 预处理运算符在 C++ 和 ANSI/ISO C 中都是可用的。

\# 运算符会把 replacement-text 令牌转换为用引号引起来的字符串

```cpp
#include <iostream>  
using namespace std;  
  
#define MKSTR( x ) #x  
  
int main ()  
{  
    cout << MKSTR(HELLO C++) << endl;  
    return 0;  
}

// HELLO C++
```

\## 运算符用于连接两个令牌

```cpp
#include <iostream>  
using namespace std;  
  
#define CONCAT(x, y) x ## y  
  
int main() {  
    int DAY = 24;  
    cout << CONCAT(DA, Y);  // cout << DAY;  
}
```

如果必须要换行，则再末尾加上 `\`

```cpp    
#define MAX(a,b) ((a)>(b) ? (a) \
: (b))
```

#### C++ 中的预定义宏

C++t提供一些预定义宏

|宏|描述|
|:--:|:--|
| `__LINE__`|这会在程序编译时包含当前行号|
|`__FILE__`|这会在程序编译时包含当前文件名|
|`__DATE__`|这会包含一个形式为 month/day/year 的字符串，它表示把源文件转换为目标代码的日期|
|`__TIME__`|这会包含一个形式为 hour:minute:second 的字符串，它表示程序被编译的时间|

```cpp
#include <iostream>  
using namespace std;  
  
int main ()  
{  
    cout << "Value of __LINE__ : " << __LINE__ << endl;  
    cout << "Value of __FILE__ : " << __FILE__ << endl;  
    cout << "Value of __DATE__ : " << __DATE__ << endl;  
    cout << "Value of __TIME__ : " << __TIME__ << endl;  
  
    return 0;  
}
```



### 信号处理


### 多线程

头文件：`pthread.h`

多线程是多任务处理的一种特殊形式，多任务处理允许让电脑同时运行两个或两个以上的程序。一般情况下，两种类型的多任务处理：**基于进程和基于线程**。

-   基于进程的多任务处理是程序的并发执行。
-   基于线程的多任务处理是同一程序的片段的并发执行。

#### 创建线程

下面的程序，我们可以用它来创建一个 POSIX 线程：

```cpp
#include <pthread.h>
pthread_create (thread, attr, start_routine, arg);
```


|参数|说明|
|:--|:--|
|thread|指向线程标识符指针|
|attr|一个不透明的属性对象，可以被用来设置线程属性。可以指定线程属性对象，也可以使用默认值 NULL|
|start_routine|线程运行函数起始地址，一旦线程被创建就会执行|
|arg|运行函数的参数。它必须通过把引用作为指针强制转换为 void 类型进行传递。如果没有传递参数，则使用 NULL|

创建线程成功时，函数返回 0，若返回值不为 0 则说明创建线程失败

#### 终止线程

```cpp
#include <pthread.h>
pthread_exit (status)
```

在这里，**pthread_exit** 用于显式地退出一个线程。通常情况下，pthread_exit() 函数是在线程完成工作后无需继续存在时被调用。

向函数传递参数
```cpp
#include "iostream"  
using namespace std;  
#include "pthread.h"  
  
#define NUM_THREADS 5   // 线程数量  
  
void * func(void *i) {  
    cout << *((int*)i) << endl;   // 无类型指针转为 int 指针再取值  
    pthread_exit(NULL);  
    return nullptr;  
}  
  
int main() {  
    int index[NUM_THREADS];  
    // 定义线程 id 变量  
    pthread_t tids[NUM_THREADS];  
    for (int i = 0; i < NUM_THREADS; i++) {  
        index[i] = i;  
        // 线程 id 、线程参数、调用函数、传入参数  
        int ret = pthread_create(&tids[i], NULL, func, (void *)&index[i]);  
        if (ret) {  
            cout << "error：" << ret << endl;  
            exit(-1);  
        }  
    }  
    pthread_exit(NULL);  
    return 0;  
}
```

可以通过结构体或类的形式传递
```cpp
#include <iostream>  
#include <cstdlib>  
#include <pthread.h>  
  
using namespace std;  
  
#define NUM_THREADS 5  
  
struct thread_data {  
    int thread_id;  
    char *message;  
};  
  
void *PrintHello(void *threadarg) {  
    struct thread_data *my_data;  
  
    my_data = (struct thread_data *) threadarg;  
  
    cout << "Thread ID : " << my_data->thread_id;  
    cout << " Message : " << my_data->message << endl;  
  
    pthread_exit(NULL);  
    return 0;
}  
  
int main() {  
    pthread_t threads[NUM_THREADS];  
    struct thread_data td[NUM_THREADS];  
    int rc;  
    int i;  
  
    for (i = 0; i < NUM_THREADS; i++) {  
        cout << "main() : creating thread, " << i << endl;  
        td[i].thread_id = i;  
        td[i].message = (char *) "This is message";  
        rc = pthread_create(&threads[i], NULL, PrintHello, (void *) &td[i]);  
        if (rc) {  
            cout << "Error:unable to create thread," << rc << endl;  
            exit(-1);  
        }  
    }  
    pthread_exit(NULL);  
}
```

#### 连接和分离线程

```cpp
pthread_join (threadid, status)   // 连接线程 
pthread_detach (threadid)
```

pthread_join() 子程序阻碍调用程序，直到指定的 threadid 线程终止为止。当创建一个线程时，它的某个属性会定义它是否是可连接的（joinable）或可分离的（detached）。只有创建时定义为可连接的线程才可以被连接。如果线程创建时被定义为可分离的，则它永远也不能被连接

```cpp
#include <iostream>  
#include <pthread.h>  
#include <unistd.h>  
  
using namespace std;  
  
#define NUM_THREADS 5  
  
void *wait(void *t) {  
    int i;  
    long * tid;  
  
    tid = (long* ) t;  
  
    sleep(1);  
    cout << "Sleeping in thread " << endl;  
    cout << "Thread with id : " << *tid << "  ...exiting " << endl;  
    pthread_exit(NULL);
    return 0;  
}  
  
int main() {  
    int rc;  
    int i;  
    pthread_t threads[NUM_THREADS];  
    pthread_attr_t attr;  
    void *status;  
  
    // 初始化并设置线程为可连接的（joinable）  
    pthread_attr_init(&attr);  
    pthread_attr_setdetachstate(&attr, PTHREAD_CREATE_JOINABLE);  
  
    for (i = 0; i < NUM_THREADS; i++) {  
        cout << "main() : creating thread, " << i << endl;  
        rc = pthread_create(&threads[i], NULL, wait, (void *) &i);  
        if (rc) {  
            cout << "Error:unable to create thread," << rc << endl;  
            exit(-1);  
        }  
    }  
  
    // 删除属性，并等待其他线程  
    pthread_attr_destroy(&attr);  
    for (i = 0; i < NUM_THREADS; i++) {  
        rc = pthread_join(threads[i], &status);  
        if (rc) {  
            cout << "Error:unable to join," << rc << endl;  
            exit(-1);  
        }  
        cout << "Main: completed thread id :" << i;  
        cout << "  exiting with status :" << status << endl;  
    }  
  
    cout << "Main: program exiting." << endl;  
    pthread_exit(NULL);  
}
```


#### std::thread

C++ 11 之后添加了新的标准线程库 std::thread，std::thread **在 `<thread>`** 头文件中声明，因此使用 std::thread 时需要包含 **在 `<thread>`** 头文件

std::thread 默认构造函数，创建一个空的 **std::thread** 执行对象

```cpp
#include<thread>
using namespace std;
thread thread_object(callable)
```


一个可调用对象可以是以下三个中的任何一个：

-   函数指针
-   函数对象
-   lambda 表达式

```cpp
#include <iostream>  
#include <thread>  
  
using namespace std;  
  
// 一个虚拟函数  
void foo(int Z) {  
    for (int i = 0; i < Z; i++) {  
        cout << "线程使用函数指针作为可调用参数\n";  
    }  
}  
  
// 可调用对象  
class thread_obj {  
public:  
    void operator()(int x) {  
        for (int i = 0; i < x; i++)  
            cout << "线程使用函数对象作为可调用参数\n";  
    }  
};  
  
void test() {  
    cout << "线程 1 、2 、3 "  
            "独立运行" << endl;  
  
    // 函数指针  
    thread th1(foo, 3);  
  
    // 函数对象  
    thread th2(thread_obj(), 3);  
  
    // 定义 Lambda 表达式  
    auto f = [](int x) {  
        for (int i = 0; i < x; i++)  
            cout << "线程使用 lambda 表达式作为可调用参数\n";  
    };  
  
    // 线程通过使用 lambda 表达式作为可调用的参数  
    thread th3(f, 3);  
      
    // 等待线程完成  
    // 等待线程 t1 完成  
    th1.join();  
    // 等待线程 t2 完成  
    th2.join();  
    // 等待线程 t3 完成  
    th3.join();  
}  
  
int main() {  
    test();  
    return 0;  
}


// 线程 1 、2 、3 独立运行
// 线程使用函数指针作为可调用参数
// 线程使用函数指针作为可调用参数
// 线程使用函数指针作为可调用参数
// 线程使用函数对象作为可调用参数
// 线程使用函数对象作为可调用参数
// 线程使用函数对象作为可调用参数
// 线程使用 lambda 表达式作为可调用参数
// 线程使用 lambda 表达式作为可调用参数
// 线程使用 lambda 表达式作为可调用参数
```