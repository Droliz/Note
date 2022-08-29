# Go | 青训营笔记
这是我参与「第三届青训营 -后端场」笔记创作活动的的第1篇笔记

## 语言结构

一个基本的 go 代码

```go
package main  // 定义包名为 main，必须要指定包名
  
import "fmt"  // 导入 fmt 包（实现了格式化 io ），如果要导入多个可以用小括号（）包裹

// 程序开始执行的函数（主函数）
func main() {  // { 是不能单独占一行
   /* 这是一个多行注释 */
   // 这是一个单行注释  
   fmt.Println("Hello, World!")  // 输出语句，自动换行
   fmt.print("Hello, World!")    // 输出语句，不换行
}
```

go 文件的运行有两种方式，直接运行，生成二进制文件运行

```go
go run "GO_FILE_PATH"  // 执行 go 文件


go bulid "GO_FILE_PATH" //生成二进制文件
./FILE_NAME  // 执行二进制文件
```

## 基础语法

### 数据类型

|类型|描述|
|:--:|:--|
|布尔型|布尔型的值只能是true和false|
|数字类型|整型int、浮点型float32、float64，go也支持复数类型，其中的位运算采用补码|
|字符串类型|string|
|派生类型|指针类型、数组类型、结构化类型、Channel类型、函数类型、切片类型、接口类型、Map类型|

### 变量

#### 变量命名

**使用`var`关键字命名**

```go
// 多变量声明（赋值）
var NAME_1, NAME_2…… [TYPE] [= VALUE_1, VALUE_2……]
```

```go
// 单变量声明（赋值）
var NAME [TYPE] [= VALUE]
```

```go
// 一般用于声明全局变量
var (
NAME_1 TYPE
NAME_2 TYPE
……
)
```

类型和赋值至少有一个

示例

```go
package main

import (

    "fmt"

)

func main() {

    var a int = 10

    var b, c int = 20, 30

    fmt.Println(a, b, c)  

}

// 输出结果
// 10 20 30
```

**只声明不赋值**

* 数值类型为 `0`
* 布尔类型为`false`
* 字符串类型为`""`
* 以下类型皆为`nil`
	*  `*int`
	* `[]int`
	* `map[]string int`
	* `chan int`
	* `func(string) int`
	* `error`（接口）

```go
package main

import (

    "fmt"

)

func main() {

    var a int

    var b string

    var c bool

    var d error

    var e *int

    fmt.Println(a, b, c, d, e)

}

// 输出
// 0 "" false <nil> <nil>

// b 是一个空字符串
```

**使用赋值操作符**

可以直接使用`:=`实现变量的声明和赋值，但是这个变量不能提前声明（编译器自动推断数据类型），而且这个初始化声明只能使用一次，再次赋值直接用赋值符号`=`

```go
NAME_1, NAME_2 …… := VALUE_1, VALUE_2 ……
```

```go
package main

import (

    "fmt"

)

func main() {

    a := 1
    
    var b int 
    
    b = 1

	fmt.println(a, b)

}

// 输出
// 1 1
```

其中如果是整数默认为`int`，浮点数默认为`float64`

```go
package main

import (

    "fmt"

)

func main() {

    a := 12

    b := 2.1

    fmt.Println("%T", a)

    fmt.Println("%T", b)

}

// 输出
// int
// float64
```

#### 值类型与引用类型

**值类型**

所有像 int、float、bool 和 string 这些基本类型都属于值类型，使用这些类型的变量直接指向存在内存中的值

值类型中使用赋值语句`a = b`赋值使，就相当于将`a`的值复制给`b`

**引用类型**

更复杂的数据通常会需要使用多个字，这些数据一般使用引用类型保存

一个引用类型的变量 a 存储的是 b 的值所在的内存地址（数字，被称为指针），或内存地址中第一个字所在的位置

当使用`a = b`赋值时，相当于将 `b` 的地址指向 `a` 的值，当`a`的值发生变化时，`b`的值也会发生变化



### 常量

#### 常量赋值

**使用const关键字**

常量中的值是不可以被修改的

常量中的数据类型只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型

```go
// Type 和赋值至少有一个

// 单变量
const NAME [TYPE] [= VALUE]

// 多变量
const NAME_1, NAME_2…… [TYPE] [= VALUE_1, VALUE_2……]

// 多变量如果指定类型，那么这多个变量都只能是同一个类型
```

```go
package main

import (
    "fmt"
)

func main() {

    const a = "Hello"      

    a = "World"       // error:cannot assign to a (untyped string constant "Hello")

    fmt.Println(a)
    
}
```

**特殊常量**

iota：可以认为是一个可以被编译器修改的常量

iota 在 const关键字出现时将被重置为 0(const 内部的第一行之前)，const 中每新增一行常量声明将使 iota 计数一次

```go
package main

import "fmt"

func main() {

    const (

        a = iota // 0

        b        // 1

        c        // 2

        d = "ha" // 独立值，iota += 1

        e        // "ha"   iota += 1

        f = 100  // iota += 1

        g        // 100  iota += 1

        h = iota // 7  恢复计数

        i        // 8

    )

    fmt.Println(a, b, c, d, e, f, g, h, i)

}


// 输出
// 0 1 2 ha ha 100 100 7 8
```


### 变量作用域

**局部变量**

定义在函数体内的，作用域只在函数之内

```go
package main

import "fmt"

func main() {

    res := "是一个局部变量，作用域在函数main中"

    fmt.Printf("res%s\n", res)

}

// 输出
// res是一个局部变量，作用域在函数main中
```

**全局变量**

在函数外定义的变量。可以在整个包使用，也可以在包外使用

```go
package main

import "fmt"

var res = "是一个全局变量，可以在任何地方被访问"

func main() {

    fmt.Printf("res%s\n", res)

}

// 输出
// res是一个全局变量，可以在任何地方被访问
```

**形式参数**

在函数定义中的变量，一般当中函数的局部变量

```go
package main

import "fmt"

func main() {

    res := test(10, 20)

    fmt.Print(res)

}

// x,y作为函数test的形式参数
func test(x, y int) int {

    if x < y {

        return y

    } else {

        return x

    } 

}

// 输出
// 20
```

### 循环语句

go支持for循环，但是没有while循环

```go
package main

import "fmt"

func main() {

    var name int = 2  

    test: for true {

        if name == 1 {

            fmt.Printf("break退出整个循环\n")

            break

        } else if name == 2 {

            fmt.Printf("continue跳过当前循环\n")  

            name--

            continue

        }

        goto test  

    }

}

// 输出
// continue跳过当前循环
// break退出整个循环
```

### 条件语句

##### 判断语句

go支持如下的条件判断语句：`if`、`if else`、`if 多层嵌套`、`switch`、`select`

**select语句**

`select`语句与`switch`类似，但是不同的是

每个 case 必须是一个通信操作，要么是发送要么是接收。select 随机执行一个可运行的 case。如果没有 case 可运行，它将阻塞，直到有 case 可运行。一个默认的子句应该总是可运行的

```go
select {  
    case communication clause  :  
       statement(s);        
    case communication clause  :  
       statement(s);  
    /* 你可以定义任意数量的 case */  
    default : /* 可选 */  
       statement(s);  
}
```

以下描述了 select 语句的语法：

-   每个 case 都必须是一个通信
-   所有 channel 表达式都会被求值
-   所有被发送的表达式都会被求值
-   如果任意某个通信可以进行，它就执行，其他被忽略。
-   如果有多个 case 都可以运行，Select 会随机公平地选出一个执行。其他不会执行。  
    否则：
    1.   如果有 default 子句，则执行该语句。
    2.  如果没有 default 子句，select 将阻塞，直到某个通信可以运行；Go 不会重新对 channel 或值进行求值。

**示例：**

```go
package main

import "fmt"

func main() {

   var c1, c2, c3 chan int  // 双向型 chan

   var i1, i2 int

   select {

      case i1 = <-c1:

         fmt.Printf("received ", i1, " from c1\n")

      case c2 <- i2:

         fmt.Printf("sent ", i2, " to c2\n")

      case i3, ok := (<-c3):  // same as: i3, ok := <-c3

         if ok {

            fmt.Printf("received ", i3, " from c3\n")

         } else {

            fmt.Printf("c3 is closed\n")

         }

      default:

         fmt.Printf("no communication\n")

   }    

}

// 输出
// no communication
```

**switch语句**

go语言的switch语句和其他语言类似

```go
package main

import (
    "fmt"
    "time"
)
  
func main() {
    a := 2
    switch a {
    case 1:
        fmt.Println("one")
    case 2:
        fmt.Println("two")
    case 3:
        fmt.Println("three")
    case 4, 5:
        fmt.Println("four or five")
    default:
        fmt.Println("other")
    }
  
    t := time.Now()
    switch {
    case t.Hour() < 12:
        fmt.Println("It's before noon")
    default:
        fmt.Println("It's after noon")
    }
}

// 输出
// two
// It's after noon
```

### 数组

**数组定义**

```go
var NAME [SIZE][SIZE]……[SIZE] TYPE 
NAME := [SIZE][SIZE]……[SIZE]TYPE{val, val, val……}
// 如果不确定大小可以用...代替
```

如果在定义数组的时候没有指定大小，编译器会自行根据数组长度推断

初始化一个数组
```go
NAME = [SIZE]TYPE{index:val, index:val……}

// 没有进行初始化的索引，值会取该数据类型的默认值
```

**示例**

```go
package main

import "fmt"

func main() {
    var a [5]int
    a[4] = 100
    fmt.Println("get:", a[2])
    fmt.Println("len:", len(a))
    b := [5]int{1, 2, 3, 4, 5}
    fmt.Println(b)
    var twoD [2][3]int
    for i := 0; i < 2; i++ {
        for j := 0; j < 3; j++ {
            twoD[i][j] = i + j
        }
    }
    fmt.Println("2d: ", twoD)
}

// 输出
// get: 0
// len: 5
// [1 2 3 4 5]
// 2d:  [[0 1 2] [1 2 3]]
```

### 切片（Slice）

Go 数组的长度不可改变，在特定场景中这样的集合就不太适用，Go 中提供了一种灵活，功能强悍的内置类型切片("动态数组")，与数组相比切片的长度是不固定的，可以追加元素，在追加时可能使切片的容量增大

切片本身是不存储任何数据的，只是对底层数组的一个引用

**定义切片**

```go
var NAME []type
```

使用make函数创建切片

```go
NAME := make([]type, len, capacity)
```

* len：切片长度
* capacity：切片的容量

除了类型，长度len和容量cap都是可以省略的（其中len<=cap）

**切片初始化**

```go
// 直接初始化切片，int类型，初始化值为1, 2 ,3，其中len = cap = 3
s := [] int {1, 2, 3}

// 通过数组初始化切片
s := arr[start_index: end_index]

// 通过make函数
s := make([]int, len, cap)
```


**len()和cap()**

切片是可索引的，并且可以由 len() 方法获取长度。

切片提供了计算容量的方法 cap() 可以测量切片最长可以达到多少

```go
package main  

import "fmt"  

func main() {  

   var numbers = make([]int, 3, 5) 
    
   printSlice(numbers)  

}  

func printSlice(x []int){ 

   fmt.Printf("len=%d cap=%d slice=%v\n", len(x), cap(x), x)  

}

// 输出
// len=3 cap=5 slice=[0 0 0]
```

**空切片**

一个切片在未初始化之前默认为 nil，长度为 0

```go
package main  
  
import "fmt"  
  
func main() {  

   var numbers []int  
  
   printSlice(numbers)  
  
   if(numbers == nil){  
   
      fmt.Printf("切片是空的")  
      
   }  
}  
  
func printSlice(x []int){ 

   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)  
}

// 输出
// len=0 cap=0 slice=[]
// 切片是空的
```

**切片截取**

```go
package main  
  
import "fmt"  
  
func main() {  
   // 初始化切片  
   s := []int{1, 2, 3, 4, 5, 6, 7, 8, 9}  
  
   // 切片的长度和容量  
   fmt.Printf("len = %d cap = %d s = %v\n", len(s), cap(s), s)  
  
   // 切片的切片  
   s1 := s[1:3]  
  
   // 切片的长度和容量  
   fmt.Printf("len = %d cap = %d s[1:3] = %v\n", len(s1), cap(s1), s1)  
}

// 输出
// len = 9 cap = 9 s = [1 2 3 4 5 6 7 8 9]
// len = 2 cap = 8 s[1:3] = [2 3]
```

**append()和copy()**

如果想增加切片的容量，我们必须创建一个新的更大的切片并把原分片的内容都拷贝过来

```go
package main  
  
import "fmt"  
  
func main() {  
   // 定义一个切片  
   var slice []int  
   fmt.Printf("len = %d cap = %d slice = %v\n", len(slice), cap(slice), slice)  
   
   // 追加元素  
   slice = append(slice, 1, 2, 3)  
   fmt.Printf("len = %d cap = %d slice = %v\n", len(slice), cap(slice), slice)  
   
   // 追加切片  
   slice = append(slice, []int{4, 5, 6}...)  
   fmt.Printf("len = %d cap = %d slice = %v\n", len(slice), cap(slice), slice)  
   
   // 复制切片  
   slice2 := make([]int, len(slice), (cap(slice)) * 2)  
   fmt.Printf("len = %d cap = %d slice2 = %v\n", len(slice2), cap(slice2), slice2)  
   copy(slice2, slice)  
   fmt.Printf("len = %d cap = %d slice2 = %v\n", len(slice2), cap(slice2), slice2)  
}
```

**示例**

```go
package main

import "fmt"

func main() {

    s := make([]string, 3)

    s[0] = "a"

    s[1] = "b"

    s[2] = "c"

    fmt.Println("get:", s[2])   // c

    fmt.Println("len:", len(s)) // 3

    s = append(s, "d")

    s = append(s, "e", "f")

    fmt.Println(s) // [a b c d e f]

    c := make([]string, len(s))

    copy(c, s)

    fmt.Println(c) // [a b c d e f]

    fmt.Println(s[2:5]) // [c d e]

    fmt.Println(s[:5])  // [a b c d e]

    fmt.Println(s[2:])  // [c d e f]

    good := []string{"g", "o", "o", "d"}

    fmt.Println(good) // [g o o d]

}
```

### 集合（map）
Map 是一种无序的键值对的集合。Map 最重要的一点是通过 key 来快速检索数据，key 类似于索引，指向数据的值

```go
/* 声明变量，默认 map 是 nil */
var map_variable map[key_data_type]value_data_type

/* 使用 make 函数 */
map_variable := make(map[key_data_type]value_data_type)
```

**示例**

```go
package main

import "fmt"

func main() {
    m := make(map[string]int)
    m["one"] = 1
    m["two"] = 2
    fmt.Println(m)           // map[one:1 two:2]
    fmt.Println(len(m))      // 2
    fmt.Println(m["one"])    // 1
    fmt.Println(m["unknow"]) // 0
  
    r, ok := m["unknow"]
    fmt.Println(r, ok) // 0 false

    delete(m, "one")

    m2 := map[string]int{"one": 1, "two": 2}
    var m3 = map[string]int{"one": 1, "two": 2}
    fmt.Println(m2, m3)
}
```

### 范围（range）

Go 语言中 range 关键字用于 for 循环中迭代数组(array)、切片(slice)、通道(channel)或集合(map)的元素。在数组和切片中它返回元素的索引和索引对应的值，在集合中返回 `key-value` 对

遍历可迭代元素，`key`为索引，`value`为对应的值，如果不需要，那么可以用 `_` 代替 

```go
for key | _, value | _ := range item {
	CODE
}
```

**示例**

```go
package main

import "fmt"
  
func main() {
    nums := []int{2, 3, 4}
    sum := 0
    for i, num := range nums {
        sum += num
        if num == 2 {
            fmt.Println("index:", i, "num:", num) // index: 0 num: 2
        }
    }
    fmt.Println(sum) // 9
  
    m := map[string]string{"a": "A", "b": "B"}
    for k, v := range m {
        fmt.Println(k, v) // b 8; a A
    }
    for k := range m {
        fmt.Println("key", k) // key a; key b
    }
}
```
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           
### 函数

```go
// 定义函数
func FUN_NAME([VAR_1 type, VAR_2 type……]) [return_type_1, return_type_2……] {
	CODE
	[return val_1, val_2……]
}
```

在传入的参数中如果都是同类型，可以只在末尾写参数类型

```go
package main

import "fmt"

func main() {
   /* 定义局部变量 */
   var a int = 100

   var b int = 200

   var ret int

   /* 调用函数并返回最大值 */
   ret = max(a, b)

   fmt.Printf( "最大值是 : %d\n", ret )

}

/* 函数返回两个数的最大值 */
func max(num1, num2 int) int {
   /* 定义局部变量 */
   var result int

   if (num1 > num2) {

      result = num1

   } else {
      result = num2
   }

   return result

}
```


### 指针

一个指针变量指向了一个值的内存地址

指针的声明
```go
var NAME *var_type
```

**指针的使用**

```go
package main

import "fmt"

func main() {

    // 定义一个int类型指针

    var ptr *int

    a := 10

    // 将a的地址赋值给ptr

    ptr = &a

    fmt.Printf("%t", a == *ptr)

}

// 输出
// true
```

**空指针**

如果一个指针被定义但是没有分配变量，那么值为`nil`，也称为空指针

```go
package main

import "fmt"

func main() {

    // 定义一个int类型指针

    var ptr *int

	fmt.Printf("%x\n", ptr)    // 指针的值为 0 

    fmt.Printf("%t\n", ptr == nil)

}

// 输出
// 0
// true
```

### 结构体

一旦定义了结构体类型，它就能用于变量的声明，如果在声明变量时，结构体中某个元素没有设置值，那么会给定为该类型的默认值

```go
package main

import "fmt"

// 定义一个结构体

type person struct {

	name string

	age  string

}

func main() {

    // 创建一个结构体变量
    p := person{name: "张三", age: "12"}
    // 查看name属性值
    fmt.Println(p.name)
    fmt.Println(p)
}

// 输出
// 张三
// {张三 12}
```

**结构体方法**

```go
package main

import "fmt"

func add(a int, b int) int {
    return a + b
}
  
func add2(a, b int) int {
    return a + b
}
  
func exists(m map[string]string, k string) (v string, ok bool) {
    v, ok = m[k]
    return v, ok

}

func main() {
    res := add(1, 2)
    fmt.Println(res) // 3
    v, ok := exists(map[string]string{"a": "A"}, "a")
    fmt.Println(v, ok) // A True

}
```


### 错误处理

在go中，对于错误的处理相比其他语言更加的直观，能够很快的找到错误

```go
package main
import (
    "errors"
    "fmt"
)

type user struct {
    name     string
    password string
}

func findUser(users []user, name string) (v *user, err error) {
    for _, u := range users {
        if u.name == name {
            return &u, nil
        }
    }
    return nil, errors.New("not found")
}


func main() {
    u, err := findUser([]user{{"wang", "1024"}}, "wang")
    if err != nil 
        fmt.Println(err)
        return
    }

    fmt.Println(u.name) // wang

    if u, err := findUser([]user{{"wang", "1024"}}, "li"); err != nil {
        fmt.Println(err) // not found
        return
    } else {
        fmt.Println(u.name)
    }
}
```

### 字符串操作

**字符串的基本操作**

在go中sting包有很多的字符串操作，包括但不限于：计数、查找字符位置、判断是否存在字符，输出长度等

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    a := "hello"
    fmt.Println(strings.Contains(a, "ll"))                // true
    fmt.Println(strings.Count(a, "l"))                    // 2
    fmt.Println(strings.HasPrefix(a, "he"))               // true
    fmt.Println(strings.HasSuffix(a, "llo"))              // true
    fmt.Println(strings.Index(a, "ll"))                   // 2
    fmt.Println(strings.Join([]string{"he", "llo"}, "-")) // he-llo
    fmt.Println(strings.Repeat(a, 2))                     // hellohello
    fmt.Println(strings.Replace(a, "e", "E", -1))         // hEllo
    fmt.Println(strings.Split("a-b-c", "-"))              // [a b c]
    fmt.Println(strings.ToLower(a))                       // hello
    fmt.Println(strings.ToUpper(a))                       // HELLO
    fmt.Println(len(a))                                   // 5
    b := "你好"
    fmt.Println(len(b)) // 6
}
```

**字符串格式化**

```go
package main  
  
import "fmt"  
  
type point struct {  
   x, y int  
}  
  
func main() {  
   s := "hello"  
   n := 123  
   p := point{1, 2}  
   fmt.Println(s, n) // hello 123  
   fmt.Println(p)    // {1 2}  
  
   fmt.Printf("s=%v\n", s)  // s=hello  
   fmt.Printf("n=%v\n", n)  // n=123  
   fmt.Printf("p=%v\n", p)  // p={1 2}  
   fmt.Printf("p=%+v\n", p) // p={x:1 y:2}  
   fmt.Printf("p=%#v\n", p) // p=main.point{x:1, y:2}  
  
   f := 3.141592653  
   fmt.Println(f)          // 3.141592653  
   fmt.Printf("%.2f\n", f) // 3.14  
}
```

### json处理

在结构体中的保证每个字段的第一个字母大写，就可以使用 `json.Marshal()` 方法来序列化，可以用`json.Unmarshal()`反序列化

```go
package main  
  
import (  
   "encoding/json"  
   "fmt")  
  
type userInfo struct {  
   Name  string  
   Age   int `json:"age"`  
   Hobby []string  
}  
  
func main() {  
   a := userInfo{Name: "wang", Age: 18, Hobby: []string{"Golang", "TypeScript"}}  
   buf, err := json.Marshal(a)  
   if err != nil {  
      panic(err)  
   }  
   fmt.Println(buf)         // [123 34 78 97...]  
   fmt.Println(string(buf)) // {"Name":"wang","age":18,"Hobby":["Golang","TypeScript"]}  
  
   buf, err = json.MarshalIndent(a, "", "\t")  
   if err != nil {  
      panic(err)  
   }  
   fmt.Println(string(buf))  
  
   var b userInfo  
   err = json.Unmarshal(buf, &b)  
   if err != nil {  
      panic(err)  
   }  
   fmt.Printf("%#v\n", b) // main.userInfo{Name:"wang", Age:18, Hobby:[]string{"Golang", "TypeScript"}}  
}
```

### 时间处理

利用time包可以实现很多的时间处理，比如获取现在的时间`time.Now()`，`time.Unix()`获取时间

```go
package main  
  
import (  
   "fmt"  
   "time")  
  
func main() {  
   now := time.Now()  
   fmt.Println(now) // 2022-03-27 18:04:59.433297 +0800 CST m=+0.000087933  
   t := time.Date(2022, 3, 27, 1, 25, 36, 0, time.UTC)  
   t2 := time.Date(2022, 3, 27, 2, 30, 36, 0, time.UTC)  
   fmt.Println(t)                                                  // 2022-03-27 01:25:36 +0000 UTC  
   fmt.Println(t.Year(), t.Month(), t.Day(), t.Hour(), t.Minute()) // 2022 March 27 1 25  
   fmt.Println(t.Format("2006-01-02 15:04:05"))                    // 2022-03-27 01:25:36  
   diff := t2.Sub(t)  
   fmt.Println(diff)                           // 1h5m0s  
   fmt.Println(diff.Minutes(), diff.Seconds()) // 65 3900  
   t3, err := time.Parse("2006-01-02 15:04:05", "2022-03-27 01:25:36")  
   if err != nil {  
      panic(err)  
   }  
   fmt.Println(t3 == t)    // true  
   fmt.Println(now.Unix()) // 1648738080  
}
```

### 数字解析

利用go语言的strconv包，实现对数字的解析

**示例**

```go
package main  
  
import (  
   "fmt"  
   "strconv")  
  
func main() {  
   f, _ := strconv.ParseFloat("1.234", 64)  
   fmt.Println(f) // 1.234  
  
   n, _ := strconv.ParseInt("111", 10, 64)  
   fmt.Println(n) // 111  
  
   n, _ = strconv.ParseInt("0x1000", 0, 64)  
   fmt.Println(n) // 4096  

   n2, _ := strconv.Atoi("123")  
   fmt.Println(n2) // 123  
  
   n2, err := strconv.Atoi("AAA")  
   fmt.Println(n2, err) // 0 strconv.Atoi: parsing "AAA": invalid syntax  
}
```

### 进程信息

go中可以通过os包查看进程信息，常用的`os.Args`获取传入的参数

```go
package main  
  
import (  
   "fmt"  
   "os"   "os/exec")  
  
func main() {  
   // go run example/20-env/main.go a b c d  
   fmt.Println(os.Args)           // [/var/folders/8p/n34xxfnx38dg8bv_x8l62t_m0000gn/T/go-build3406981276/b001/exe/main a b c d]  
   fmt.Println(os.Getenv("PATH")) // /usr/local/go/bin...  
   fmt.Println(os.Setenv("AA", "BB"))  
  
   buf, err := exec.Command("grep", "127.0.0.1", "/etc/hosts").CombinedOutput()  
   if err != nil {  
      panic(err)  
   }  
   fmt.Println(string(buf)) // 127.0.0.1       localhost  
}
```










