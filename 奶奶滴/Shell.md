# Shell

## Shell 教程

shell文件后缀可以是sh（代表shell）也可以是php这不影响程序的运行

在sh文件顶部添加 #!/bin/bash 告诉操作系统用来==解释脚本的程序位置==（执行时也手动输入，就不用写）

### Shell 与 Shell 脚本

Shell 本身是一个用**C 语言编写**的程序，是一个命令行解释器，它的作用就是遵循一定的语法将输入的命令加以解释并传给系统，它是用户使用 Linux 的桥梁，是UNIX/Linux系统的**用户与操作系统之间的一种接口**

Shell既是一种命令语言，又是一种程序设计语言(shell脚本)

### 运行 Shell 脚本的方法

* Linux中
    * 先给权限`chmod u+x ./test.sh`然后执行`./test.sh`（==需要权限，每次都会开启一个新的进程运行脚本==）
    * 直接运行`bash ./test.sh`（==不需要权限，新进程执行脚本==）
    * `. test.sh`（==不需要权限，不会开启新进程脚本==）

* windows中
    * 依靠git bash，可以直接运行

## Shell 变量

#### 命名规则
* 只能使用英文字母、数字、下划线
* 不能以数字开头
* 中间不能有空格
* 不能是bash关键字

#### 定义变量
赋值等号两边不能有空格

* **局部变量**：局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量
* **环境变量**：所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量
* **shell变量**：shell变量 shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行

**declare定义一个有类型的变量**

`declare [+/-][rxi][var_name=var_value]`或`declare -f`

* +/-："-"表示指定变量的属性值，"+"表示取消变量设置的属性值
* rxi：r：设置为只读；x：设置为环境变量，可供shell以外的程序使用；i：可以是数值、字符串或运算式
* -f：只显示函数

```sh
# 定义一个变量（可被更改）
declare -i a=10
# 也可以直接定义
a=10

declare -x b="这是一个环境变量"   
# 引用变量需要 $ 符号   {}标识变量的范围，单独输出时可省略
echo ${a}

# 定义一个只读变量（不可被更改）
readonly c='这是一个只读变量'
declare -r d="这也是一个只读变量"

# 声明数组变量
declare -a arr='([0]="a" [1]="b" [2]="c")' 

# 如果不取消属性直接赋值
a="wor" # a原本是10属性是数值，现再直接赋值会变为0
# 需要先取消赋值
declare +i a
a='wor'	# a='wor'

# 删除变量
unset a
unset b
unset c     # 报错，不可被更改
```

>如果变量指定了类型，再赋值其他类型，不会报错，但是会清空（整数变为0字符串变为空等）

#### 交互式变量的定义
`read [参数] 变量名`

|参数|说明|
|:--:|:--|
|-p|定义提示信息|
|-n|定义长度|
|-s|不显示输入内容|
|-t|定义输入时间，默认单位为秒|

```sh
# 获取来自用户的值
# 在 10 秒内输入长度最多为 5 的不显示字符
read -s -t 10 -n 5 -p "输入变量" var

# 在后面添加 < 文件路径 表示来源于文件（带文件名的路径）
```

#### Shell 字符串

Shell中的字符串可以是双引号也可以时单引号，但是这两者也是有着去别的

##### 字符串的定义
```sh
# 单引号
var_name=10
# 单引号''，单引号会把内容原样输出，即便转义也没用，也不支持输出变量的内容
echo '这个数字是 ${var_name} 小于100'   # 这个数字是 ${var_name} 小于100   
# 双引号""，支持转义和变量的输出
echo "这个数字是 ${var_name} 小于100"   # 这个数字是 10 小于 100
```

##### 获取字符串的长度
```sh
a="abcd"
echo ${#a}      # 4
```

##### 提取子字符串
```sh
# string_name: start: length
string="Hello Word!"
echo ${string: 1: 4}    # ell0
```

##### 查找子字符串
```sh
# `expr index ${string} index_name`（反引号`）
string="time is now"
# 查找 t 和 i 谁先出现输出谁
echo `expr index "${string}" ti`  # 0

# ``反引号：用于做命令替换

# expr命令：可以实现数值运算、字符串匹配、字符串比较、字符串提取、字符串长度、计算等功能，还有判断变量或参数是否为整数、为空、为0等
```

#### 注释
```sh
# 单行注释

:<<EOF
多行注释
多行注释
EOF
# EOF可以被其他符号代替，如！、'等
```

## Shell 传递参数
向shell文件中传递参数`bash test.sh 参数1 参数2`

```sh
# 在sh文件中 ${0} 代表此文件的路径（包括文件名）从 1 开始代表第几个传入的参数
${0}
${1}
${2}
```

|参数处理|说明|
|--|:--|
|$ n |n>=1 传递的第n个参数|
|$ # |传递到脚本或函数的参数个数|
|$ * |传递给脚本或函数的所有参数|
|$ $ |脚本运行的当前进程ID号|
|$ ! |后台运行的最后一个进程的ID号| 
|$ @ |与$ * 相同，但是加引号使用时有所不同|
|$ - |显示Shell使用的当前选项，与[set命令](https://www.runoob.com/linux/linux-comm-set.html)功能相同|
|$ ? |显示==最后命令的退出状态或函数返回值==。0表示没有错误，其他任何值表明有错误|

## Shell 数组

shell的数组==只能支持一维数组==，并且==没有定义数组的大小==，而且允许每个元素类型不同

#### 定义数组
```sh
array_name=(val0, val1, val2, val3, val4, val5, val6)

# 也可以单独定义
array_name[0]=val0
```

#### 读取数组
```sh
# array_name[index] index：@、*、0-inf
${array_name[0]}    # 索引为 0 的值
${array_name[@]}    # 所有的值
${array_name[*]}	# 所有的值
```

#### 数组长度
```sh
len=${array_name[@]}    # 整个数组长度
len=${array_name[*]}    # 整个数组长度
len=${array_name[0]}    # 索引为 0 的元素长度
```


## Shell 基本运算符

Shell 和其他编程语言一样，支持多种运算符
* 算数运算符
* 关系运算符
* 布尔运算符
* 字符串运算符
* 文件测试运算符

原生的bash不支持简单的数学原酸，但是可以通过其他的命令来实现；例如 wak   和expr等`expr 2 + 2`就可以实现`2 + 2`的加法（运算式每个元素必须空格隔开）而且完整的表达式必须要反引号包裹 

#### 算数运算符

|操作符|说明|
|:--:|:--|
|+|加法|
|-|减法|
|\*|乘法|
|/|除法|
|%|取余|
|=|赋值|
|== |判断相等|
|!= |判断不相等|

```sh
#!bin/bash
a=20
b=10
echo `expr ${a} + ${b}` 
echo `expr ${a} - ${b}`
echo `expr ${a} \* ${b}`	# *必须有\
echo `expr ${a} / ${b}`
echo `expr ${a} % ${b}`
echo `expr ${a} == ${b}`
echo `expr ${a} != ${b}`
```

```
30
10
200
2
0
0
1
```

#### 关系运算符

|操作符|说明|
|:--:|:--|
|-eq|检测是否相等|
|-ne|检测是否不相等|
|-gt|检测左边是否大于右边|
|-lt|检测左边是否小于右边|
|-ge|检测左边是否大于等于右边|
|-le|检测左边是否小于等于右边|

```sh
a=10
b=20

# -eq 检测是否相等
if [ $a -eq $b ]  
then  
 echo "$a -eq $b : a 等于 b"  
else  
 echo "$a -eq $b: a 不等于 b"  
fi  
# -ne 检测是否不相等
if [ $a -ne $b ]  
then  
 echo "$a -ne $b: a 不等于 b"  
else  
 echo "$a -ne $b : a 等于 b"  
fi  
# -gt 测左边是否大于右边
if [ $a -gt $b ]  
then  
 echo "$a -gt $b: a 大于 b"  
else  
 echo "$a -gt $b: a 不大于 b"  
fi  
# -lt 测左边是否小于右边
if [ $a -lt $b ]  
then  
 echo "$a -lt $b: a 小于 b"  
else  
 echo "$a -lt $b: a 不小于 b"  
fi  
# -ge 检测左边是否大于等于右边
if [ $a -ge $b ]  
then  
 echo "$a -ge $b: a 大于或等于 b"  
else  
 echo "$a -ge $b: a 小于 b"  
fi  
# -le 检测左边是否小于等于右边
if [ $a -le $b ]  
then  
 echo "$a -le $b: a 小于或等于 b"  
else  
 echo "$a -le $b: a 大于 b"  
fi
```

```
10 -eq 20: a 不等于 b 
10 -ne 20: a 不等于 b 
10 -gt 20: a 不大于 b 
10 -lt 20: a 小于 b 
10 -ge 20: a 小于 b 
10 -le 20: a 小于或等于 b
```

#### 布尔运算符

|操作符|说明|
|:--:|:--|
|!|非运算|
|-o|或运算|
|-a|与运算|

```sh
a=10  
b=20  

# 在bool中所有为空、0的都是代表false，不为空、1都是代表true

# !：非运算
# -o：或运算
# -a：与运算

if [ $a != $b ]  
then  
 echo "$a != $b : a 不等于 b"  
else  
 echo "$a == $b: a 等于 b"  
fi  
if [ $a -lt 100 -a $b -gt 15 ]  
then  
 echo "$a 小于 100 且 $b 大于 15 : 返回 true"  
else  
 echo "$a 小于 100 且 $b 大于 15 : 返回 false"  
fi  
if [ $a -lt 100 -o $b -gt 100 ]  
then  
 echo "$a 小于 100 或 $b 大于 100 : 返回 true"  
else  
 echo "$a 小于 100 或 $b 大于 100 : 返回 false"  
fi  
if [ $a -lt 5 -o $b -gt 100 ]  
then  
 echo "$a 小于 5 或 $b 大于 100 : 返回 true"  
else  
 echo "$a 小于 5 或 $b 大于 100 : 返回 false"  
fi
```

```
10 != 20 : a 不等于 b 
10 小于 100 且 20 大于 15 : 返回 true 
10 小于 100 或 20 大于 100 : 返回 true 
10 小于 5 或 20 大于 100 : 返回 false
```

#### 逻辑运算符

|操作符|说明|
|:--:|:--|
|&&|逻辑的and|
|\|\||逻辑的or|


```sh
#!bin/bash
# && 逻辑and 
# || 逻辑or

#!bin/bash

a=20
b=10

# 由于 && 分割了两个命令，左右两边括号的分别接收作为参数
# 也可以写成[ &a -lt 100 ] && [ $b -gt 100 ]
if [[ $a -lt 100 && $b -gt 100 ]]
then
 echo "返回 true"
else
 echo "返回 false"
fi 
if [[ $a -lt 100 || $b -gt 100 ]]
then
 echo "返回 true"
else
 echo "返回 false"
fi
```

#### 字符串运算符

|操作符|说明|
|:--:|:--|
|=|检测两字符串是否相等|
|!=|检验两字符串是否不相等|
|-z|检测字符串长度是否为 0 |
|-n|检测字符串长度是否不为 0 |

```sh
#!bin/bash

a="abc"  
b="efg"  
# = 检测两字符串是否相等
if [ $a = $b ]  
then  
 echo "$a = $b : a 等于 b"  
else  
 echo "$a = $b: a 不等于 b"  
fi  
# != 检验两字符串是否不相等
if [ $a != $b ]  
then  
 echo "$a != $b : a 不等于 b"  
else  
 echo "$a != $b: a 等于 b"  
fi  
# -z 检测字符串长度是否为 0
if [ -z $a ]  
then  
 echo "-z $a : 字符串长度为 0"  
else  
 echo "-z $a : 字符串长度不为 0"  
fi  
# -n 检测字符串长度是否不为 0 
if [ -n "$a" ]  
then  
 echo "-n $a : 字符串长度不为 0"  
else  
 echo "-n $a : 字符串长度为 0"  
fi  
```


#### 文件测试运算符

|操作符|说明|
|:--:|:--|
|-b file|检测文件是否是块设备文件|
|-c file|检测文件是否是字符设备文件|
|-d file|检测文件是否是目录|
|-f file|检测文件是否是普通文件（既不是目录，也不是设备文件）|
|-g file|检测文件是否设置了 SGID 位|
|-k file|检测文件是否设置了粘着位(Sticky Bit)，如果是|
|-p file|检测文件是否是有名管道|
|-u file|检测文件是否设置了 SUID 位|
|-r file|检测文件是否可读|
|-w file|检测文件是否可写|
|-x file|检测文件是否可执行|
|-s file|检测文件是否为空|
|-e file|检测文件（包括目录）是否存在|

```sh
#!bin/bash

file="file_path"  
if [ -r $file ]  
then  
 echo "文件可读"  
else  
 echo "文件不可读"  
fi  
if [ -w $file ]  
then  
 echo "文件可写"  
else  
 echo "文件不可写"  
fi  
if [ -x $file ]  
then  
 echo "文件可执行"  
else  
 echo "文件不可执行"  
fi  
if [ -f $file ]  
then  
 echo "文件为普通文件"  
else  
 echo "文件为特殊文件"  
fi  
if [ -d $file ]  
then  
 echo "文件是个目录"  
else  
 echo "文件不是个目录"  
fi  
if [ -s $file ]  
then  
 echo "文件不为空"  
else  
 echo "文件为空"  
fi  
if [ -e $file ]  
then  
 echo "文件存在"  
else  
 echo "文件不存在"  
fi
```


## Shell echo与printf

### Shell echo命令

```sh
# shell中的echo命令用于字符串输出（默认换行）
echo string
# 参数 -e：开启转义
# \n:换行
# \c:不换行
```

### Shell printf命令

```sh
# 不会默认换行
printf format-string [arguments...]
```

* format-string：格式控制字符串
	* %-ns：输出n个字符串
	* %-nd：输出n个整数（ni也可以）
	* %-m.nf：输出m为整数n为小数的浮点数
	* %c：输出字符
* arguments：参数列表

```sh
#!bin/bash

# format-string为双引号
printf "%d %s\n" 1 "abc"

# 单引号与双引号效果一样
printf '%d %s\n' 1 "abc" 

# 没有引号也可以输出
printf %s abcdef

# 格式只指定了一个参数，但多出的参数仍然会按照该格式输出，format-string 被重用
printf %s abc def
printf "%s\n" abc def
printf "%s %s %s\n" a b c d e f g h i j

# 如果没有 arguments，那么 %s 用NULL代替，%d 用 0 代替
printf "%s and %d \n"
```
	
	
## Shell test命令
检测某个条件是否成立，可以进行**数值、字符、文件**的测试

### 数值测试
```sh
#!bin/bash

# 数值的操作都可以
if test 1 -eq 0;
then
 echo "true"
else
 echo "false"
fi

# 在之前的代码中遇到的[]执行基本的算术运算
```

### 字符串测试
```sh
#!bin/bash

# 字符串的操作都可以
if test "abs" = "abs";
then
	echo "两个字符串相等"
else
	echo "两个字符串不相等"
fi
```

### 文件测试
```sh
#!bin/bash

# 文件操作都可以
if test -e ./test;
then
	echo "文件已存在"
else
	echo "文件不存在"
fi
```


## Shell 流程控制

shell的流程控制不能为空，即要么不写，要么写了就必须有内容

### if else-if fi
```sh
#!bin/bash
a=1

if [ ${a} == 1 ];
then
 echo "1"
elif [ ${a} == 0 ];
then
 echo "0"
else
 echo "2"
fi
```

### for 循环
```sh
for (var; var<=n; var ++);
do
	printf "%s\n" 这里是循环代码
done

# 无限循环
for (( ; ; ))
```

### for in 循环
```sh

# value_list 是数值列表，in value_list可以省略相当于 in $@
for var in value_list
do
	代码块
done
```

```sh
# $(seq 起始 步长 结束)	生产范围的数
for n in $(seq 0 1 10)	# 也可以是`seq 0 1 10`

for filename in $(ls *.sh)	# 列出以.sh结尾的所有文件名 *.sh 也可

for i in 1 2 3 4 # 直接写在后面用空格隔开

for str in {1..100}	# 1-100		{A..z} A-z

# 还有特殊变量 $#、$*、$@、$?、$$等（传递参数）
for i in $@
```

### while 语句
```sh
#!bin/bash
i=0
while (( i < 5 ))
do 
	echo $i
	let "i++"	
done

# while也可以用于读取键盘输入的信息
echo "按下<CTRL-D>退出循环"
while read FILM
do 
 echo "键盘输入的内容是"
done

# 无限循环的两种写法
while true
while :
```

>let 命令是bash中用于计算的工具，用于执行一个或多个表达式，==计算中不用加上$表示变量的引用==，如果包含空格和其他==特殊字符==，必须要引起来

### until 循环
until循环与while循环相反，==只有条件为假才会进入循环，条件为真就退出==

```sh
#!bin/bash

i=1
until (( i > 5 ))
do
 echo $i
 let "i++"
done
```


### case …… esac

多选择语句，类似switch……case 语句类似，* 号代表不属于上述的所有，;;代表结束

```sh
# 每个分支结束用 ;; 代表 break
case 值 in
模式1)
	代码块
	;;
模式2)
	代码块
	;;
*)
	代码块
	;;
esac
```

```sh
#!bin/bash
num=1
case $num in
	1)	echo '1'
	;;
	2)	echo '2'
	;;
	3) 	echo '3'
	;;
	4|5|6)	echo '456'
	;;
	*)	echo '都不是'
	;;
esac
```

### 跳出循环

#### break
跳出所有的循环，不会继续后面的循环
#### continue
跳出当前这一轮的循环，还会继续后面的循环
 
 
## shell 函数

```sh
[ function ] funname [()]
{
	# 代码块
	
	[ return ]
}
funname vla1 vla2 vla3 ……
```

```sh
#!bin/bash
funname() {
	echo "这是一个函数"
	echo "传入的参数是 $1"
	i=0
	for ((i; i < 5; i++));
	do
		printf "%d\n" $i
	done
}

funname '函数'
```

## 输入输出重定向
|命令|说明|
|:--:|:--|
|conmand > file|将输出重定向到file|
|conmand < file|将输入重定向到file|
|conmand >> file|将输出以==追加==的形式重定向到file|
|n > file|将文件描述符为 n 的文件重定向到file|
|n >> file|将文件描述符为 n 的文件以追加的形式重定向到file|
|n >& m|将输出文件 m 和 n 合并|
|n <& m|将输入文件 m 和 n 合并|
|<< tag|将开始标记tag和结束标记tag之间的内容作为输入|

>文件描述符 0 通常是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）
> \>代表的是覆盖，\>\>代表的是追加

```sh
# 输出重定向
$ who > users	# 将命令的完整输出重定向到用户文件中users

# 输入重定向
wc -l < test.sh	# wc用于统计文件的行数、单词数和字符数等

# 输出输入重定向
command < infile > outfile	# 从文件infile读取内容，然后将输出写入到outfile
```

### Here Document
Here Document 是 Shell 中的一种特殊的重定向方式，用来将输入重定向到一个交互式 Shell 脚本或程序

```sh
command << delimiter
	document
delimiter

# 结尾的delimiter 一定要顶格写，前面不能有任何字符，后面也不能有任何字符，包括空格和 tab 缩进。
# 开始的delimiter前后的空格会被忽略掉

cat << EOF
	重定向
EOF
```

>如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 `/dev/null`即`command > /dev/null`


## 文件包含

Shell也可以包含外部脚本

```sh
. file_path	# .与文件名之间有空格
source filename_path # source也可

# 包含的文件会直接执行
```


## shell 的括号
### $()和\`\`
都可以用于命令替换
*  \`\`基本上可用于全部的shell中，如果写成shell script，移植性比较高
*  $()不是所有的shell都支持

```sh
# 获取版本号
version=$(uname -r)
varsion=`uname -r`
```

### ${}
用于变量替换，一般来说`$var`和`${var}`并没有区别，但是${}更加的精确变量名称的范围

```sh
A=a
AB=b
echo $AB	# 这样引用的是AB	b
echo ${A}B	# 范围就是A所以	AB
```

#### ${}模式匹配功能

* \#：去掉左边的
* %：去掉右边的

> \#和%中的单一符号是最小匹配，两个相同符号是最大匹配

```sh
${variable#patten}	# 查找variable
```

## Linux 命令大全
[Linux命令大全](https://www.runoob.com/linux/linux-command-manual.html)

