# go数组、切片、集合、范围

## 数组

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

## 切片（Slice）

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

## 集合（map）
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

## 范围（range）

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