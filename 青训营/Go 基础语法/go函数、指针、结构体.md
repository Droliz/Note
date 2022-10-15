# go函数、指针、结构体


## 函数

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


## 指针

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

## 结构体

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