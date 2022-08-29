# UE4 C++ 输出

## 宏 UE_LOG

UE_LOG 是专门用于在控制台中输出日志信息的宏 

例如：

```cpp
FString cpp = "c++";
UE_LOG(LogTemp, Display, TEXT("Hello UE4 %s"), cpp);
```

FString 是相当于 c++ 中的 string 类型