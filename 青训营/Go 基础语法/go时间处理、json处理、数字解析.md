# go时间处理、json处理、数字解析

## json处理

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

## 时间处理

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

## 数字解析

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
