# 画图库

```python
import turtle  # 画图库  
  
def Test():  
    # 设置窗口宽高，以及窗口位置  
    turtle.setup(800, 600, 0, 0)  
    turtle.pensize(10)   # 画笔大小  
    turtle.pencolor("yellow")   # 画笔颜色  
    turtle.fillcolor("red")   # 填充颜色  
    turtle.speed(9)   # 画笔速度 0~10  
    turtle.begin_fill()   # 开始填充  
    for _ in range(5):  
        turtle.forward(200)   # 画笔方向前进   backward 向后  
        turtle.right(144)   # 顺时针旋转    left 逆时针  
    turtle.end_fill()   # 填充结束  
  
    turtle.penup()   # 不会绘制   pendown  绘制  
    turtle.goto(200, 200)   # 转到点坐标  
    turtle.color("violet")   # 同时设置画笔颜色和填充颜色  
  
    # 文本  
    turtle.write("Done", font=('Arial', 40, 'normal'))  
    # 等待操作（持续显示）  
    turtle.mainloop()  
  
  
if __name__ == "__main__":  
    Test()  
    pass
```

### 画笔运动
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220829215352.png)

### 画笔控制

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220829215359.png)

### 全局控制

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220829215431.png)

### 其他

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220829215449.png)