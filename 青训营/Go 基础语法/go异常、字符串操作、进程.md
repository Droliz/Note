# go异常、字符串操作、进程

## 错误处理

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

## 字符串操作

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


## 进程信息

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