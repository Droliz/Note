# go循环与条件

## 循环语句

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

## 条件语句

### 判断语句

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
