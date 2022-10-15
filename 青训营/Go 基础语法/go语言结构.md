# go语言结构

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