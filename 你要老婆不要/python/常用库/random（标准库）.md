# random库

random库是用于生成随机数的

```python
import random  
  
def Test():  
    # 随机浮点数  
    print(random.uniform(0, 1))  
    # 随机整数  
    print(random.randint(0, 100))  
    print(random.random())   # 0~1 范围浮点数  
    # 随机偶数    步长 2    print(random.randrange(0, 100, 2))  
    # 随机字符  
    print(random.choice(["1", "a", "b"]))  
    print(random.sample("asdasdas", 3))  # 随机选 3 个，返回数组  
    # 随机洗牌  
    a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]  
    random.shuffle(a)  
    print(a)  
  
  
if __name__ == "__main__":  
    Test()  
    pass
```