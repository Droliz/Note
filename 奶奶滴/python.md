# python
## python 基础
### 公共(列表、元组、字典、字符串通用)
#### 1、python内置函数
```
    len(item)       计算容器中元素个数
    del(item)       删除变量（有两种方式 del item）
    max(item)       返回容器元素最大值（如果是字典比较 key）
    min(item)       返回容器元素最小值（如果是字典比较 key）
    iter(item)      迭代器（如果是字典，迭代键）
    all()           依次做比较，返回 bool ，只有全真才返回 True
```
#### 2、切片（字典不能）
```    
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
```
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

>元组应用
```
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

```
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

>***常用***：
```
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

```
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

* 1、类和对象
   * 类：一群育有相同特征或行为的事物（抽象的）       类是一个特殊的对象
       * 特征->属性
       * 行为->方法
       * 三要素：类名（大驼峰不用下划线）、属性（对特征描述）、方法（具有的行为）
   * 对象：由类创造出来的具体存在（具体的）
       * ***类只有一个，但对象可以有很多个***，不同对象之间属性可能不同
       * 类中定义的属性和方法，创建出来的对象就具有相应的属性和方法
       * `dir()`查看对象内的所有属性
       * __两个下划线开头和结尾的都是针对对象内置方法/属性
   * 类的定义：
        ```
        class 类名：
            def 方法1 (self, 参数列表)
                pass
            def 方法1 (self, 参数列表)
                pass
        ```
   * 创建对象：\
        对象变量 = 类名()    \
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
           * 父类方法不能满足子类需求，再次调用时只会调用子类重写的方法\
               * 1、在子类中重写（定义一个同名的方法）
               * 2、在子类中重写时，在需要的位置使用super().父类方法，来调用父类方法
               * 3、父类可以访问自己的私有方法
       * 多继承：
            >子类可以继承多个父类，拥有所有父类的属性和方法(父类之间有同名的属性和方法，应该避免多继承(利用MRO查看))\
            class 子类名(父类名1， 父类名2，...)\
        `__MRO__`：可以查看方法搜索顺序\
            `类名.__mro__`
        
   * 多态：不同的子类，调用相同的父类方法，产生不同的执行效果
    
   * 类对象：
       * 1、类属性： 记录和类相关的特征 \   
            定义：类属性名 = 值（在类中）   在初始化方法中改变\
            调用：类名.类属性名
       * 2、类方法： \
            `@classmethod`\
            `def 类方法名(cls):`      
            在方法内部可以使用cls.调用类属性或其他类方法\
            静态方法：\
                在既不需要访问实例属性也不需要调用实例方法，也不访问类属性，调用类方法时可以封装成静态方法\
            `@staticmethod`\
            `def 静态方法名():`
                
    >实例方法————方法内部需要访问实例属性
         实例方法内部可以使用 类名. 访问类属性
    类方法————方法内部只需要访问类属性
    静态方法————方法内部，不需要访问实例属性和类属性
    
   * 单列：\
        单列设计模式    \
            让类创建的对象，在系统中只有唯一的一个实例
            每一次返回的对象，内存地址是相同的\
            `__new__方法`:\
                由object提供的内置静态方法,为内存中对象分配空间,返回对象的引用\
                重写new方法一定要返回(固定)：\
                `return super().__new__(cls)`\
            定义一个类属性为False   可以使初始化方法只调用一次        
"""

---
### 错误和异常
```
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
    
```
import 模块名 as 模块别名（模块名太长时，更改模块名，方便记忆）

from 模块名 import 工具名     （只导入一部分工具)
__name__在模块中执行时会输出__main__，而在导入时不是__main__
if __name__ == "__main__"
```
        
包：  特殊的目录，包含多个模块，目录下包含一个__init__.py文件\
    import 包名    可以导入包里所有模块  \
    需要在__init__中导入模块：\
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
       * open(文件名, 访问模式)     默认在当前文件夹下打开或创建\
        `f = open("test.txt", mode='w', encoding='utf-8)`
        用上述方法打开文件，需要关闭\
        `with open("test.text", mode='w', encoding='utf-8') as fp: `        用这种方法打开文件不用再调用关闭
    * 2、关闭
        `close()`\
        `f.close()`   
>常用
```    
f.read(n)       读取 n 个字符，执行一次就继续向后读
f.readlines(hint = -1)   默认读取整个文件   hint = n   n 限制行数
f.write
.strip()        去除中间空格
f.flush()       强制保存
```
* 导入os模块实现文件/目录管理操作
   * 文件：
        ```
        os.rename(原文件名， 目标文件名)   重命名文件
        os.remove(文件名)   删除文件
        ```
   * 目录：
        ```
        os.listdir(目录名)       目录列表
        os.mkdir(目录名)         创建目录
        os.rmdir(目录名)         删除目录
        os.getcwd()             获取当前目录
        os.chdir(目标目录)        修改工作目录
        os.path.isdir(文件路径)   判断是否是文件 
        ```
* 读取 csv 文件名为 wdo
    ```
    import csv
    with open("wdo.csv", "r", encoding="utf-8") as fp:
        reader = csv.reader(fp)
        for row in reader:
            print(row)
    ```
* 读取 xlsx 文件
    ```
    import xlrd
    xlsx = xlrd.open_workbook('001.xlsx')
    sheet = xlsx.sheet_by_index(0)
    ```

---
### TODO 拓展

* 1、位运算：
    ```
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

    `from collections import defaultdict` 
    `dict = defaultdict(factory_function)`        
    >这个factory_function可以是list、set、str等等，作用是当key不存在时，返回的是工厂函数的默认值，比如list对应[ ]，str对应的是空字符串，set对应set( )，int对应0
    
    如下举例：
    ```
    dict1 = defaultdict(int)        0
    dict2 = defaultdict(set)        set()
    dict3 = defaultdict(str)        ''
    dict4 = defaultdict(list)       []
    dict5 = defaultdict(dict        {}
    ```
    例如：\
        如果要计算列表里的元素出现的次数就可以用 int 来完成
        
        from collections import defaultdict 
        dic = defaultdict(int)
        li = [1, 1, 1, 3, 3, 2, 2, 2]
        for i in li:
        将要统计的元素作为键, int 下默认值为 0 故每出现一次 key 对应的 val就加一,最后字典的值就是元素出现的次数
            dic[i] += 1         

### 区间合并
1、按区间左端点排序\
2、扫描整个区间，将有交集的区间合并

## Python数据结构与算法
### 二分查找

>O(log(n))\
要求***有序列表***，必须要*先排序*\
如果是无序列表，而且查找值比较少，不建议二分查找
![二分查找](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg-blog.csdnimg.cn%2F20210222214705314.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4OTI5NDMz%2Csize_16%2Ccolor_FFFFFF%2Ct_70&refer=http%3A%2F%2Fimg-blog.csdnimg.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644703955&t=7171fb209b7ca7506d0c68bed0d35ce6)
```
def cha(arr: list[int], key: int) -> int:
    """
    二分查找，
    先将被查找的列表排序（升降都可）
    找中间的数值与查找的比较，如果相等即返回
    如果大于就在左边数列继续上面的操作
    如果小于就在右边数列继续上面的操作
    :param arr:被查找的数列
    :param key:要查找的数
    :return:要查找的数的索引
    """
    i = 0
    high = len(arr) - 1
    while i <= high:
        # 取中间的一个值,防止溢出
        mid = i + (high - i) // 2
        if arr[mid] == key:
            return mid
        # 如果mid的数大于要查找的数就在 i ~ mid - 1之间找mid重复操作
        elif arr[mid] > key:
            n = mid - 1
        # 如果mid的数小于要查找的数就在 mid + 1 ~ high 之间找mid重复操作
        elif arr[mid] < arr:
            i = mid + 1
```
>递归二分查找
```
def cha(nums: list[int], target: int, start: int, end: int):
    start = 0
    end = len(arr) - 1
    mid = (start + end) // 2

    if arr[mid] == target:
        return mid
    elif arr[mid] > target:
        return cha(arr, target, start, mid - 1)
    elif arr[mid] < target:
        return cha(arr, target, mid + 1, end)
```
### ***排序***
![排序算法](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimage.mamicode.com%2Finfo%2F201810%2F20181017232651322830.jpg&refer=http%3A%2F%2Fimage.mamicode.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644704615&t=ac8ea5b73cfd14cb1678dce3974d5193)
### 1、冒泡排序
>复杂度较高\
>如果在有一趟中没有发生任何交换可以认为已经排好了
![冒泡排序](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimage.mamicode.com%2Finfo%2F201911%2F20191104201241366155.png&refer=http%3A%2F%2Fimage.mamicode.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644704280&t=f5f42c13a0e22293d089fc497a476552)
```
def maopao(nums: list[int]):
    """"
    冒泡排序
    比较两个相邻的数，如果不符合需要的排序（升序或降序）则交换两数
    冒泡排序一共会运行len（nums）-1趟（最后一趟不用比）
    每循环一趟会出来一个符合排序规则的数（在有序区）
    每次循环只在无序区排序
    有序去范围:i + 1
    无序区范围:len(nums) - i - 1     （第 i 趟）
    :param nums:要排序的列表
    :return:拍好的列表
    """
    # 第 i 趟
    for i in range(len(nums)-1):
        exchange = False  # 优化，当没有交换一次的时候直接返回
        # 在无序区范围排序
        for j in range(len(nums)-i-1):
            # 比较相邻的两个数，不符合排序规则的交换
            if nums[j] > nums[j+1]:  # 大于号改为小于号即为降序
                nums[j], nums[j+1] = nums[j+1], nums[j]
        if not exchange:
            return nums
            # print("第 %d 趟：" % i, nums)
    return nums
```

### 2、选择排序    
![选择排序](https://img2.baidu.com/it/u=3364014278,802276825&fm=253&fmt=auto&app=138&f=GIF?w=480&h=452)
```
def xuanze(nums: list[int]):
    """
    假定第一个数为最小值，如果在后面发现更小的则交换，
    然后在无序区重复以上操作
    :param nums: 要排序的数列
    :return: 排序好的数列
    """
    # 需要 len(nums) - 1 趟
    for i in range(len(nums) - 1):
        min_loc = i       # 假定无序区第一个位置的数是最小值
        # 无序区的范围
        for j in range(i+1, len(nums)):
            # 当发现j位置的数比假定的小就交换
            if nums[j] < nums[min_loc]:
                min_loc = j     # 将j位置设为最小
        nums[i], nums[min_loc], = nums[min_loc], nums[i]
    return nums
```

### 3、插入排序    
![插入排序](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg.136.la%2F20210718%2Fe728ce7200624a7d89fe33ede1969550.jpg&refer=http%3A%2F%2Fimg.136.la&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644705160&t=ab3ca55c5b7b132bff912b0c2af6f844)
```
def charu(nums: list[int]):
    # i 表示要排序的下标
    for i in range(1, len(nums)):
        tmp = nums[i]
        j = i - 1   # 排好的下标
        # 要排的比排好的最右边的数大而且没有比到最左边
        while nums[j] > tmp and j >= 0:
            nums[j+1] = nums[j]     # 向左移一位
            j -= 1
        nums[j+1] = tmp
    return nums
```

### 4、快速排序
![快速排序](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fbkimg.cdn.bcebos.com%2Fpic%2F574e9258d109b3dee4ddfa6acfbf6c81800a4c55&refer=http%3A%2F%2Fbkimg.cdn.bcebos.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644705453&t=affa05f48f52e6fc3bb5ba53a433705d)
```
def kuaisu(nums: list[int], left: int, right: int) -> list[int]:
    # 保存要排序的最左边的数
    tmp = nums[left]
    while left < right:
        while nums[right] >= tmp and left < right:
            right -= 1
        nums[left] = nums[right]
        while nums[right] <= tmp and left < right:
            left += 1
        nums[left] = nums[right]

    nums[left] = tmp

    return nums
```
***递归快速排序***
```
def kuaipai(nums: list[int], left: int, right: int) -> list[int]:
    # temp = nums[left]
    if left < right:
        # mid = kuaisu(nums, left, right)
        kuaipai(nums, left, right-1)
        kuaipai(nums, left+1, right)

    return nums
```

### 5、归并排序
>1- 确定分界点
2- 递归排序 left right
3- 归并 —— 合二为一   
![归并排序](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimage.mamicode.com%2Finfo%2F201905%2F20190513091756164239.png&refer=http%3A%2F%2Fimage.mamicode.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644705541&t=1ea092d46a612ce4d8da19a1de3a7ea6)
```python
def guibing(nums: list[int], left: int, right: int) -> list[int]:
    mid = int((left+right) // 2)
    j = mid + 1
    tmp = []    # 用于存储排序好的
    k = 0       # 记录次数
    while left <= mid and j <= right:

        if nums[left] <= nums[j]:
            k += 1
            left += 1
            tmp[k] = nums[left]

    return nums
```

## 6、堆排序
### 基本知识
#### 1、树
* 树是一种可递归定义的数据结构
* 树是由n个节点组成的集合（递归定义）：
    >如果n = 0，那这是一颗空树\
    >如果n > 0，那么存在1个节点作为根节点，其他节点分为m个集合，每个集合本身又是棵树
#### 基本概念
* 1、根节点、子节点、叶子节点：树的最顶端为根节点，根节点往下分为子节点，直到后面没有子节点的节点为叶子节点
* 2、树的深度（高度）：根节点到最下一层的层数
* 3、树的度：这个节点有几个子节点，度就是几，节点的最大度就是树的度
* 4、孩子节点、父节点：节点是子节点的父节点，子节点是节点的孩子节点
* 5、子树：一个树的一个分支都是子树
#### 2、二叉树
* 度不超过 2 的树就是二叉树（每个节点最多只有两个孩子节点，分别为左孩子节点和右孩子结点）
* 满二叉树：如果每层的节点数都到达了最大值
* 完全二叉树：叶子节点只能出现在最下层和次下层，并且最下层的节点都集中在最左边的若干位置
#### 二叉树的存储方式（表示方式）
* 链式存储方式：用链表表示
* 顺序存储方式：用列表表示，从根节点开始为 0 ，按层往下，从左往右依次增加
  >父节点编号与左孩子节点编号 i -> 2i + 1，与左孩子节点 i -> 2i + 2
### 堆排序
* 1、堆：一种特殊的完全二叉树
  >大根堆：一颗完全二叉树，满足任一节点都比其他孩子节点大\
  >小根堆：一颗完全二叉树，满足任一节点都比其他孩子节点小
![堆](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg-blog.csdnimg.cn%2F2021042907094577.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoZW5sb25nX2N4eQ%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70&refer=http%3A%2F%2Fimg-blog.csdnimg.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644703328&t=f6aa7ed18ad21aac5d1848748c1bec29)
* 2、堆的向下调整性质
  >假设根节点的左右子树都是堆，但跟节点不满足堆的性质，那么可以通过一次向下的调整来将其变成一个堆
* 3、堆的向下取整
  >拿去根节点，将孩子节点中较大的补上去，依次重复操作，直到拿出的根节点的数有放入的空位
  ![堆的向下取整](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg-blog.csdnimg.cn%2F2020112819400047.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NQZW5fd2Vi%2Csize_16%2Ccolor_FFFFFF%2Ct_70&refer=http%3A%2F%2Fimg-blog.csdnimg.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644703328&t=9b935ec60a1911686ed123ea4723eeda)

#### 堆排序
* 1、建立堆
* 2、得到堆顶元素为最大元素
* 3、去掉堆顶，将堆最后一个元素放到堆顶，此时可通过一次调整重新使堆有序
* 4、堆顶元素为第二大元素
* 5、重复步骤 3 ，直到堆变空\

***向下取整函数实现***

```python
def sift(li: list[int], low: int, high: int):
	"""
	:param li: 要排序的数组
	:param low: 堆的根节点位置
	:param high: 堆的最后一个元素的位置
	"""
	i = low         # i 初始化指向根节点
	j = 2 * i + 1   # j 初始化为根节点的左孩子结点 
	tmp = li[i]     # 将堆顶保存起来

	# 只要 j 的位置不为空(没超过最后一个节点位置)
	while j <= high:
		# 如果右孩子存在且有孩子大于左孩子
		if li[j + 1] > li[j] and j + 1 <= high:
			j = j + 1   # 指向右孩子
		# 比较 tmp 和 li[j]（较大的孩子节点），较大的放到堆顶
		if li[j] > tmp:
			li[i] = li[j]
			i = j           # 更新 i 的位置（往下看一层）
			j = 2 * i + 1   # 更新 j 的位置（新 i 的左孩子节点）
		else:       # tmp 更大，将 tmp 放到 i 的位置上
			li[i] = tmp
			break
	else:       # 当 j 大于 high 时跳出循环，此时叶子节点为空，将 tmp 放到 i 位置
		li[i] = tmp
``` 

***堆排序函数的实现***

```python
def heap_sort(li: list[int]) -> list[int]:
	n = len(li)
	# 从最后一个开始向前建堆
	for i in range((n - 2) // 2, -1, -1):
		# i 表示建堆的时候调整的部分的根的下标
		sift(li, i, n - 1)
	for i in range(n - 1, -1, -1):
		li[0], li[i] = li[i], li[0]
		
```


### 动态规划
* 分类
   * 基础问题：爬楼梯、斐波那契数列
   * 背包问题
   * 打家劫舍
   * 股票问题
   * 子序列问题

* 解题步骤
   * 1、dp数组以及下标 i（ j ） 的含义
   * 2、递推公式
   * 3、dp数组如何初始化
   * 4、遍历顺序
   * 5、打印dp数组

>打印杨辉三角

| 行\列 |   0   |   1   |   2   |   3   |   4   |
| :---: | :---: | :---: | :---: | :---: | :---: |
|   0   |   1   |       |       |       |       |
|   1   |   1   |   1   |       |       |       |
|   2   |   1   |   2   |   1   |       |       |
|   3   |   1   |   3   |   3   |   1   |       |
|   4   |   1   |   4   |   6   |   4   |   1   |

* 1、二维dp数组，i，j代表第i行第j列的数字
* 2、`dp[i][j] = dp[i-1][j-1] + dp[i-1][j]`
* 3、初始化0，1行和0列和[i][i]为1，故直接初始化全部为1
* 4、由图和递推公式，很显然先放列再放行，故先循环行，后循环列，且row从2 开始 n 结束，col从1开始 row-1 结束
* 5、打印dp数组
    ```
    def generate(numRows: int) -> List[List[int]]:
            # 将每一行每一个值初始化为1
            dp = [[1] * i for i in range(1, numRows + 1)]

            # 第i行的中间（2 ~ i-1）位置的值
            for i in range(2, numRows):

                for j in range(1, i):
                    dp[i][j] = dp[i-1][j-1] + dp[i-1][j]

            return dp
    ```

