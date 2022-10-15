# go数据类型、变量、常量

## 数据类型

|类型|描述|
|:--:|:--|
|布尔型|布尔型的值只能是true和false|
|数字类型|整型int、浮点型float32、float64，go也支持复数类型，其中的位运算采用补码|
|字符串类型|string|
|派生类型|指针类型、数组类型、结构化类型、Channel类型、函数类型、切片类型、接口类型、Map类型|

## 变量

### 变量命名

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

### 值类型与引用类型

**值类型**

所有像 int、float、bool 和 string 这些基本类型都属于值类型，使用这些类型的变量直接指向存在内存中的值

值类型中使用赋值语句`a = b`赋值使，就相当于将`a`的值复制给`b`

**引用类型**

更复杂的数据通常会需要使用多个字，这些数据一般使用引用类型保存

一个引用类型的变量 a 存储的是 b 的值所在的内存地址（数字，被称为指针），或内存地址中第一个字所在的位置

当使用`a = b`赋值时，相当于将 `b` 的地址指向 `a` 的值，当`a`的值发生变化时，`b`的值也会发生变化



## 常量

### 常量赋值

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


## 变量作用域

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
