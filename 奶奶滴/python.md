# python
## python 基础
### 公共(列表、元组、字典、字符串通用)
#### 1、python内置函数

```python
len(item)       计算容器中元素个数
del(item)       删除变量（有两种方式 del item）
max(item)       返回容器元素最大值（如果是字典比较 key）
min(item)       返回容器元素最小值（如果是字典比较 key）
iter(item)      迭代器（如果是字典，迭代键）
all()           依次做比较，返回 bool ，只有全真才返回 True
```

#### 2、切片（字典不能）

```python
range(start: stop: step)
	start:计数从start开始，默认是从0开始
	stop:计数到stop结束，但是不包括stop。
	step:步长，默认为1.
item[开始索引：结束索引：步长]
	步长代表每几个取一个
	(包含开始索引，不包含结束索引）
	[::-1]反转item（符号代表反转，1代表间隔）
	省略默认为是[开头,末尾,无间隔]
```

#### 3、运算符

```python
+                合并（除字典）
*                重复（除字典）
in               判断元素是否存在（字典判断的是 key）
not in           判断元素是否不存在（字典判断的是 key）
比较符            元素比较（除字典）
```

#### 4、完整 for 循环

```python
for 变量 in 集合：
	循环代码
	(break)
else:
	在没有通过 break 退出循环，循环结束后，会执行的代码
```

---
### 变量
不可变类型：内存中数据不可被更改
* 数字类型：int, bool,float,complex,long(2,x)
* 字符串：str
* 元组：tuple


可变类型：（内容发生变化，内存地址不变）
* 列表：list
* 字典：dic
  
>变量存储的是数据的内存地址\
>用id（）函数可以得到内存地址,地址本质上是数字,直接打印对象默认是16进制

---
### 列表
>用[]中括号定义，逗号隔开,索引从0开始\
在列表中存储多个相同类型的数据(可以存储不同数据类型)\

应用场景：
  * 在列表使用+=时，并不会先相加再赋值，本质上调用extend方法\
  * 存储相同数据类型，通过迭代遍历实现对列表里的元素进行相同的操作\
  * 输入一个列表：   （***相同类型的***）
  * 
```python
list = input("列表：").split(",")
list_use = [int(list[i]) for i in range(len(list))]
print(list_use)
```
>列表应用
```python
def list_use():
    name_list = ["c++", "c", "java", "python"]
    print(name_list)
    # 列表的取值索引
    count = name_list.index("c")
    print(count)
    # 修改
    name_list[0] = "gao shu"  # 修改指定位置的数据
    print(name_list)
    # 增加
    name_list.append("da shu ju")  # 在列表末尾添加数据
    print(name_list)

    name_list.insert(5, "wu shu")  # 在指定位置添加数据
    print(name_list)

    temp_list = ["yin yu"]  # 将temp内容添加到name中
    name_list.extend(temp_list)
    print(name_list)
    # 删除
    name_list.remove("yin yu")  # 将指定数据移除,多个数据相同时，删除第一个出现的
    print(name_list)

    name_list.pop()  # 默认删除最后一个元素，删除指定索引数据并返回
    print(name_list)

    del name_list[4]  # 删除指定索引的数据
    print(name_list)

    # name_list.clear()  # 清空列表
    # print(name_list)
    # del name_list
    # 从内存中将name_list删除，后续代码不可使用
    # 不建议使用del删除数据

    # 统计
    print(len(name_list))  # 获取列表元素个数
    print(name_list.count("python"))  # 查询数据出现的次数

    # 排序
    print(name_list.reverse())  # 翻转列表
    print(name_list.sort())  # 升序排序（默认）    sort会改变原始列表，用sorted(name_list)函数排序不会改变原始函数
    name_list.sort(reverse=True)  # 降序排序

    # 循环遍历
    """
    迭代遍历
    for 储存变量 in 需要遍历的列表变量
            执行语句
    """
    name_list1 = ["张三", "李四", "王五", "王小二"]
    for my_name in name_list1:
        print("我的名字叫 %s" % my_name)
```

---
### 元组

>用（）小括号定义，逗号隔开，索引从0开始\
多个不同元素组成的序列\
***元组里面的元素不可修改***\
***如果只有一个数据，在后面加个逗号***


应用场景：
* 作为函数的参数和返回值，一个函数接收任意多个参数，或返回多个数据
* 让列表不可以被修改，以保护数据安全`
a, b = b, a `交换ab数据（不用引入第三个变量）
* 用元组返回函数多个值（括号可省略）

元组应用

```python
def tuple_use():
    name_tuple = ("qtw", 20, 1.75)

    print(name_tuple[0])  # 获取0出的数据
    print(name_tuple.index("qtw"))  # 已知数据，得到数据索引
    print(name_tuple.count("qtw"))  # 得到数据在元组中出现的次数
    print(len(name_tuple))  # 得到元组中元素个数

    # 循环遍历
    for my_tuple in name_tuple:
        print(my_tuple)

    """
    元组和列表之间的转换
    元组 = list（列表）
    列表 = tuple（元组）
    """
```

---
### 字典

>无序的对象集合\
用{}大括号定义，逗号隔开\
每一个数据都有对应的唯一的键（人为定义）\
数据和索引用：冒号隔开  键（key）：值（value）\
数据可以是任何数据类型，键只能是 字符串、整数型、元组\
字典的key只能使用不可变类型的数据（是否可变见变量）\
值value可以是可变类型变量

应用场景:
* 存储描述一个物品的相关信息
* 将多个字典放在同一个列表当中，再遍历，可以得到每一个信息

```python
def dictionary_use():
    name_dic = {"name": "小明",  # 为了阅读清晰
                "age": 18,
                "gender": True,
                "height": 1.75,
                "weight": 75.5}
    # 取值
    print(name_dic["name"])  # key必须存在，否则会报错

    # 增加/修改
    name_dic["age"] = 20  # 如果key存在则更改数据，如果不存在则增加
    name_dic["gender"] = "男"

    # 删除
    name_dic.pop("gender")

    # 统计键值对数量
    print(len(name_dic))

    # 合并字典
    i_dic = {"character": "good"}
    """
    原字典.update(要合并字典)
    如果要合并字典包含原字典键值对，会覆盖原字典键值对
    """
    print(name_dic.update(i_dic))

    # 清空字典
    print(name_dic.clear())

    # 循环遍历
    # k 是获取的键
    for k in name_dic:
        print(k)
```

---
### 字符串       （文本对齐）

>***空格也算一个字符***\
\u00b2     二次方   unicode 字符串\
用''单引号，或者""双引号定义\
也可用\'和\"定义\
当字符串需要使用""时，用''定义

***常用***：

```python
以is开头的方法用于判断
1、空格判断
isspace()       判断是否只包含空格
2、数字判断       只包含数字则返回true       都不能判断小数
isdecimal()     范围：数字
isdigit()       范围：数字、unicode字符串
isnumeric()     范围：数字、Unicode字符串、中文数字
3、查找和替换
startswith("k")     检查是否以k开头（区分大小写）
endswith("k")       是否以k结尾（区分大小写）
find("k")           查找k所在的索引位置（不存在则输出-1）
replace(“旧”，“新”)  执行完成返回新字符串，但不更改原字符串
4、文本对齐
center(宽度，填充字符)        居中对齐(宽度10)默认添加字符为英文空格
ljust(宽度，填充字符)         向左对齐
rjust(宽度，填充字符)         向右对齐
5、去除空白字符、
strip()                    去除两边空白字符（r:去除右边，l去除左边）
6、拆分和链接
split()             将一个大的字符串分隔成一段一段的(默认按空格分隔)
"\*".join(string)    用*分隔，将每一个连接在一起
7、大小写
title()             返回一个标题形势的字符串(每一个单词首字母大写（单词之间要分开）)
upper()             所有字母大写
capitalize()        字符串首字母大写
```

---
### 函数
* `return (bit1, bit2,...bit)`        
   * 函数返回值可以利用元组，返回多个数据,小括号可以省略
* `gl_bit1, gl_bit2 = 函数调用`         
   * 返回值是元组时，也可以使用多个变量一次接收，以便分开处理数据(变量个数一一对应)
* `global 全局变量名`            
   * global 关键字用于在函数内部更改全局变量的值（在使用赋值语句时不会创建局部变量）\
    函数内部赋值不影响全局变量，相当于创建一个局部变量
    （如果传入时可变类型，在函数内部使用方法修改，会影响到函数外部变量）\
    列表使用+=时不会先加后赋值，而是extend方法，会更改全局变量值\
    全局变量要定义在调用函数之前，最好在最上方定义  
* `nonlocal 上一级函数变量名`  
   * 使用上一级函数的变量,不会改变上一级函数变量值
* 缺省参数：
   * 定义函数时可以给某个参数指定一个默认值 列如：列表的排序soft默认false
* 多值参数：
   * `*args`       增加一个*表示可以接收元组习惯用args
   * `**kwargs`     增加两个**可以接收字典习惯用kwargs
   * 如果需要直接将元组或者字典传入，则需要拆包
      * 拆包：再传入的元组变量前加一个*，再字典变量前加两个*
* 递归:
   * 根据接受的参数不同，处理不同的结果、
   * 当参数满足一个条件时，函数不再执行（递归的出口）

---

```python
def print_info(name, gender=True):  # 缺省参数属于定义末尾
    """
    缺省参数, 一般使用常用的作为默认值
    :param name: 姓名
    :param gender: 性别（默认为真，男）
    """
    gender = "男生"  # 默认为男生
    if not gender:
        gender = "女生"
    print("%s 是 %s" % (name, gender))
    # 调用函数时想给定特定参数值，参数=值
 
def sum_num(num):
    """
    递归的运用
    :param num:
    :return:
    """
    # 自己调用自己
    sum_num(num - 1)
    if num == 1:
        return
```

---
### 面向对象

1、类和对象

类：一群育有相同特征或行为的事物（抽象的）       类是一个特殊的对象
* 特征->属性
* 行为->方法
* 三要素：类名（大驼峰不用下划线）、属性（对特征描述）、方法（具有的行为）

对象：由类创造出来的具体存在（具体的）
* ***类只有一个，但对象可以有很多个***，不同对象之间属性可能不同
* 类中定义的属性和方法，创建出来的对象就具有相应的属性和方法
* `dir()`查看对象内的所有属性
* __两个下划线开头和结尾的都是针对对象内置方法/属性

类的定义：

```python
class 类名：
def 方法1 (self, 参数列表)
	pass
def 方法1 (self, 参数列表)
	pass
```

* 创建对象：
对象变量 = 类名()    
.属性名 = ""    在类外给对象添加属性（不推荐）    

* 内置方法：
* 初始化方法：
	`__init__ `       初始化方法:定义一个类时指定类具有那些属性（在创建对象的时候自动调用这个方法） \     
	1、将属性值以形参定义为__init__方法的参数\
	2、在方法内部使用self.属性 = 形参,接受外部传递参数\
	3、在创建对象时，使用类名(属性1，属性2...)调用
* `__del__`         对象被从内存中销毁前，会被自动调用

* `__str__`    返回一个字符串
        
>在定义属性不知道初始值时，可以定义为None
用来判断是否时None时一般用is       item is None
        
* 私有属性和方法：\
在属性和方法前加__两个下划线（那么方法和属性在类外部不能被访问，只能在内部访问）
在python中是没有真正意义上的私有\
	在私有属性和方法前 _类名 加一个下划线和类名即可访问（不建议）\
	***在子类中不能直接访问父类私有属性和方法***
* 公有属性和方法：
在外界是可以访问的\
当父类共有方法访问父类的私有方法和属性，子类不能访问父类的公有方法
	
                
* 继承：
>class 子类(父类):
子类拥有父类的所有属性和方法
* 传递性：子类的子类可以继承所有父类的方法
        
* 方法的重写：
* 父类方法不能满足子类需求，再次调用时只会调用子类重写的方法
   * 1、在子类中重写（定义一个同名的方法）
   * 2、在子类中重写时，在需要的位置使用super().父类方法，来调用父类方法
   * 3、父类可以访问自己的私有方法
* 多继承：

>子类可以继承多个父类，拥有所有父类的属性和方法(父类之间有同名的属性和方法，应该避免多继承(利用MRO查看))
class 子类名(父类名1， 父类名2，...)
`__MRO__`：可以查看方法搜索顺序
`类名.__mro__`

* 多态：不同的子类，调用相同的父类方法，产生不同的执行效果
    
* 类对象：

1、类属性： 记录和类相关的特征 
定义：类属性名 = 值（在类中）   在初始化方法中改变
调用：类名.类属性名

2、类方法： 

`@classmethod`

`def 类方法名(cls):`      

在方法内部可以使用cls.调用类属性或其他类方法

静态方法：

在既不需要访问实例属性也不需要调用实例方法，也不访问类属性，调用类方法时可以封装成静态方法

`@staticmethod`
`def 静态方法名():`

>实例方法————方法内部需要访问实例属性
实例方法内部可以使用 类名. 访问类属性
类方法————方法内部只需要访问类属性
静态方法————方法内部，不需要访问实例属性和类属性

* 单列：
单列设计模式    
	让类创建的对象，在系统中只有唯一的一个实例
	每一次返回的对象，内存地址是相同的
	`__new__方法`:
		由object提供的内置静态方法,为内存中对象分配空间,返回对象的引用
		重写new方法一定要返回(固定)：
		`return super().__new__(cls)`
	定义一个类属性为False   可以使初始化方法只调用一次        


---
### 错误和异常
```python
根据错误类型捕获异常：
try:
	尝试执行代码
except 错误类型1：
	针对错误类型1，处理的代码
except 错误类型2：
	针对错误类型2，处理的代码
except 错误类型3：
	针对错误类型3，处理的代码
except Exception as result:
	print("异常错误：%s" % result)
else:
	没有异常时执行的代码
finally:
	无论异常与否都会执行的代码
```

* 异常的传递：在函数中的异常会在调用时传递给调用者，一直到主函数，如果没有异常处理才会结束
   * 可以在主程序捕获异常
* 主动抛出异常：   （应用场景）内置的异常类\
        创建异常对象，使用`raise`关键字抛出异常

---
### 模块

以扩展名py结尾的源文件都是一个模块\
    `import 模块名`\
    `模块名.方法名`
    
```python
import 模块名 as 模块别名（模块名太长时，更改模块名，方便记忆）

from 模块名 import 工具名     （只导入一部分工具)
__name__在模块中执行时会输出__main__，而在导入时不是__main__
if __name__ == "__main__"
```
        
包：  特殊的目录，包含多个模块，目录下包含一个__init__.py文件
    import 包名    可以导入包里所有模块  
    需要在__init__中导入模块：
    form . import 模块名


---
### 文件的操作
"""
* 访问模式：    后面加b表示二进制方式打开  后面加+
   * r：以只读方式打开文件，文件指针将放在文件开头
   * w:打开文件只用于写入，如存在则覆盖，如不存在，新建
   * a:打开文件用于追加，如存在则文件指针在文件末尾，如不存在则新建文件写入
* 打开与关闭
    * 1、打开/创建
       * open(文件名, 访问模式)     默认在当前文件夹下打开或创建
        `f = open("test.txt", mode='w', encoding='utf-8)`
        用上述方法打开文件，需要关闭
        `with open("test.text", mode='w', encoding='utf-8') as fp: `        用这种方法打开文件不用再调用关闭
    * 2、关闭
        `close()`
        `f.close()`   

常用

```    
f.read(n)       读取 n 个字符，执行一次就继续向后读
f.readlines(hint = -1)   默认读取整个文件   hint = n   n 限制行数
f.write
.strip()        去除中间空格
f.flush()       强制保存
```
* 导入os模块实现文件/目录管理操作

* 文件：

```python
os.rename(原文件名， 目标文件名)   重命名文件
os.remove(文件名)   删除文件
```

* 目录：

```python
import os
os.listdir(目录名)       目录列表
os.mkdir(目录名)         创建目录
os.rmdir(目录名)         删除目录
os.getcwd()             获取当前目录
os.chdir(目标目录)        修改工作目录
os.path.isdir(文件路径)   判断是否是文件 
```

* 读取 csv 文件名为 wdo

```python
import csv
with open("wdo.csv", "r", encoding="utf-8") as fp:
	reader = csv.reader(fp)
	for row in reader:
		print(row)
```

* 读取 xlsx 文件

```python
import xlrd
xlsx = xlrd.open_workbook('001.xlsx')
sheet = xlsx.sheet_by_index(0)
```

---
### TODO 拓展

* 1、位运算：

```python
原码: x
补码: ~x + 1
反码: ~x
x = x & (x - 1)       将最右边的 1 变为 0 
x & -x = x & (~x + 1)   获取最左边 1 的位置
(n >> k) & 1         获取第 k 位是几
1 << (k - 1)         只有第k位为1的数
(1 << k) - 1         后k位为均为1的数
x >> k & 1           x 的第k+1位
x >> k | (1 << k)    x的第k+1位置1
x >> k & ~(1 << k)   x的第k+1位置0

设 f(x, y) 为x到y的所有整数的异或值。
f(1, n)  =  f(0, n)  =    
   n      n % 4 == 0
   1      n % 4 == 1
   n +1   n % 4 == 2
   0      n % 4 == 3
```


* 2、collections 运用

```python
from collections import defaultdict
dict = defaultdict(factory_function)
```    

>这个factory_function可以是list、set、str等等，作用是当key不存在时，返回的是工厂函数的默认值，比如list对应[ ]，str对应的是空字符串，set对应set( )，int对应0
   
如下举例：

```python
dict1 = defaultdict(int)        0
dict2 = defaultdict(set)        set()
dict3 = defaultdict(str)        ''
dict4 = defaultdict(list)       []
dict5 = defaultdict(dict        {}
```

例如：

如果要计算列表里的元素出现的次数就可以用 int 来完成

```python
from collections import defaultdict 
dic = defaultdict(int)
li = [1, 1, 1, 3, 3, 2, 2, 2]
for i in li:
将要统计的元素作为键, int 下默认值为 0 故每出现一次 key 对应的 val就加一,最后字典的值就是元素出现的次数
	dic[i] += 1         
```


### 区间合并
1、按区间左端点排序

2、扫描整个区间，将有交集的区间合并