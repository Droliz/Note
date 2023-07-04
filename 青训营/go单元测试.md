# 单元测试

测试命令

```sh
# 指定文件
go test -v xxx_test.go xxx.go  # -v 参数显示详细信息，例如程序中的打印等
# 在有go.mod时
go test
```

测试文件采用必须使用`_test.go`结尾，需要引入`testing`包

```go
// demo.go
package main  
  
func CalSquare() {  
   // 无缓冲的channel  
   src := make(chan int)  
   // 有缓冲的channel  
   dest := make(chan int, 3)  
   // 子协程 A 发送 0~9 到 src   go func() {  
  // 延迟资源关闭  
	defer close(src)  
	for i := 0; i < 10; i++ {  
	 src <- i  
	}  
	}()  
  
   // 子协程 B 发送 src 中数据的平方到 dest   go func() {  
      defer close(dest)  
      for i := range src {  
         dest <- i * i  
      }  
   }()  
  
   // 主协程，打印 dest 中的数据（平方）  
   for i := range dest {  
      //复杂操作  
      println(i)  
   }  
}

// demo_test.go
package main  
  
import "testing"  
  
func TestCalSquare(t *testing.T) {  
   CalSquare()  
}
```

![](../markdown_img/Pasted%20image%2020230311093334.png)


## 代码覆盖率

代码覆盖率用于评估单元测试

在使用`go test`时，加上参数`--cover`可以计算出代码的覆盖率。如果单个测试不能达到很高的覆盖率(真实项目中`100%`覆盖率基本不可能)，那么可以采用增加测试函数来达到更高

![](../markdown_img/Pasted%20image%2020230311100603.png)

