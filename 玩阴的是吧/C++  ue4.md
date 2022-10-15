# ue4

## 准备

### 下载安装

在Epic中下载安装，选择想要的版本

### 更改缓存路径

如果不更改缓存路径，所有项目将会占用 c 盘，更改缓存路径在打包发送给别人，别人会比较轻松的使用。但是更改完缓存路径，每次打开项目都要缓存

```sh
# 缓存配置文件路径
UE_4.27\Engine\Config\BaseEngine.ini

# 找到 FoldersToClean=-1, PromptIfMissing=true, Path= 
# 更改 path 内容
%GAMEDIR%DerivedDataCache

# 如果有多个版本，每个版本都要更改
```


## 项目与功能

### 项目设置

项目编写
* 蓝图
	* 蓝图项目不涉及代码，但底层都是 C++
* C++
	* C++项目由纯C++来编写

### 项目结构

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220730234614.png)

* Config：包含了项目设置，键盘输入之类的配置文件
* Content：存放引擎或游戏内容，包括地图、贴图、模型、材质、蓝图等
* Intermediate：包含在编译引擎或游戏时生成的临时文件
* Saved：包含自动保存内容，配置（\*.ini）文件以及日志文件
* DerivedDataCache：缓存文件
* .uproject：项目启动程序


### 窗口

#### 菜单

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220731002136.png)

* 文件：加载和保存项目及关卡
* 编辑：标准的复制和粘贴操作、以及编辑器首选项和项目设置
* 窗口：打开视口和其他面板
* 帮助：在线文档和教程等外部资源链接

#### 内容浏览器

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220731000141.png)

项目的地图关卡在 `内容/...BP/Maps` 下，找到关卡，点击开始即可

官方提供初学者使用的素材在 `StarterContent` 目录下，大致包括建筑模型、音频、蓝图、HDRI、关卡、材质、粒子、道具、贴图等

在添加中也可以添加新的功能和内容包

### 模板功能

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220731000703.png)

提供的蓝图模板

### 视口的移动

鼠标右键单击拖动  转动视角

鼠标右键加上 `wasd` 实现移动 加上`qe` 实现上下移动

`ctrl + alt + 左键 + 拖动` 方形选取框

`F` 聚焦到选中的 Actor 上

`alt + 左键 + 拖动`   围绕单个支点或兴趣点翻转视口

`alt + 右键 + 拖动`   向前推动摄像机使其接近或远离单个支点或兴趣点

`alt + 中键 + 拖动`   根据鼠标移动方向将摄像机向不同方向移动


### 常用文件类型

|资源类型|文件拓展名|应用程序|
|:--:|:--|:--:|
|三维动画、骨架网络体结构、动画数据|.fbx、.obj|Maya、3ds Max、ZBrush|
|纹理和图片|.bmp、.jpeg、.pcx、.png、.psd、.tga、.hdr|Photoshop|
|字体|.otf、.ttf|BitFontMaker2|
|音频|.wav|Audacity、Audition|
|视频和多媒体|.wmv|After Effects、Media Encoder|
|PhysX|.apd、.apx|APEX PhysX Lab|
|其他|.csv|Excel|


## 项目

### 项目迁移

将其他项目的资源，迁移到另一个项目中

在内容浏览器中，右键需要迁移的文件夹，选择文件夹中需要迁移的，然后选择新项目目d的`Content` 目录下（不能是 `Content` 的子文件夹）

复制粘贴也可以，但是复制过来的文件存放路径必须和被复制的文件路径一致

例如：要复制文件夹，那么一定要找到被复制项目的 `Content` 目录下的文件才行


### 项目打包

在项目根目录中、除去 `Saved、Intermediate` 文件夹，然后压缩


### BSP


## C++


### 输出

#### 记录宏 UE_LOG

```cpp
UE_LOG(LogTemp, Display, TEXT("Hello Unreal!"));   // 显示    需要通过 TEXT 转换
UE_LOG(LogTemp, Warning, TEXT("Hello Unreal!"));   // 警告
UE_LOG(LogTemp, Error, TEXT("Hello Unreal!"));     // 错误

int WeaponsNum = 4;
int killsNum = 7;
float Health = 34.45324f;
bool IsDead = false;
bool HasWeapon = true;

UE_LOG(LogTemp, Display, TEXT("weaponsNum num: %d, kills num: %i"), WeaponsNum, killsNum);
UE_LOG(LogTemp, Display, TEXT("Health :%f"), Health);
UE_LOG(LogTemp, Display, TEXT("IsDead : %d, HasWeapon : %d"), IsDead, static_cast<int>(HasWeapon));
```

#### 自定义 LOG

定义：
```cpp
DEFINE_LOG_CATEGORY_STATIC(LogMyLog, Log, All);
```

使用：
```cpp
FString customName = "jack";
UE_LOG(LogMyLog, Log, TEXT("name = %s"),*customName);
```

示例:
```cpp
// Fill out your copyright notice in the Description page of Project Settings.


#include "MyActor.h"

// 自定义
DEFINE_LOG_CATEGORY_STATIC(LogMyLog, Log, All);

// Called when the game starts or when spawned
void AMyActor::BeginPlay()
{
	Super::BeginPlay();

	FString Name = "jack";
	UE_LOG(LogMyLog, Display, TEXT("Name: %s"), *Name);   // 输出

	//printTypes();
}

```

```cpp
int KillNum = 10;
FString str = "KillNum : " + FString::FromInt(KillNum);   // 拼接

UE_LOG(LogMyLog, Display, TEXT("%s"), *str);
```

#### 在屏幕打印消息

使用全局引擎对象的指针 `GEngine` 来完成，需要包含头文件 `Engine/Engine.h`

`AddOnScreenDebugMessage` 能在屏幕打印信息

```cpp
void UEngine::AddOnScreenDebugMessage(int32 Key, float TimeToDisplay, FColor DisplayColor, const FString& DebugMessage, bool bNewerOnTop, const FVector2D& TextScale)
```

参数：
* key：-1代表只显示一次，不重复显示
* TimeToDisplay：显示时间
* FColor：字体颜色
* DebugMessage：消息内容
* bNewerOnTop：输出顺序（顶部新行，底部新行）
* TextScale：存储 （x,，y）既可以是向量也可以是坐标，比例尺

```cpp
#include "Engine/Engine.h"

FString Name = "jack";
GEngine->AddOnScreenDebugMessage(-1, 3.0f, FColor::Red, FString("Hello UE4 C++"));
GEngine->AddOnScreenDebugMessage(-1, 3.0f, FColor::Blue, Name, true, FVector2D(1.5f, 1.5f));
```

### 装饰性

#### 宏：UPROPERTY

虚幻引擎内置的整型为 int32，为避免不同平台编译问题，改为 int32

将上述代码中的变量全部放到类`AMyActor` 中，每一个变量上放使用宏`UPROPERTY()` 告诉编译器，需要在编译器中查看这些变量

```cpp
// Fill out your copyright notice in the Description page of Project Settings.

#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "MyActor.generated.h"

UCLASS()
class CPP_01_API AMyActor : public AActor {
protected:
	// Called when the game starts or when spawned
	virtual void BeginPlay() override;

	UPROPERTY(EditAnywhere)   // 使用EditAnywhere修饰类变量，默认原型和实例对象的详细面板都可编辑
	int32 WeaponsNum = 4;
	// 只能在默认原型（蓝图）中编辑，在详细面板中不能编辑
	UPROPERTY(EditDefaultsOnly, Category = "Damage")  
	int32 killsNum = 7;
	UPROPERTY(EditInstanceOnly, Category = "Damage") // 在默认原型中不可用，但在详细面板中可编辑
	float Health = 34.45324f;
	UPROPERTY(EditAnywhere, Category = "Damage")  // 默认原型和实例都可以编辑
	bool IsDead = false;
	UPROPERTY(VisibleAnywhere) // VisibleAnywhere只是让变量可见，编辑框是灰色，不能修改变量的值
	bool HasWeapon = true;
	UPROPERTY(EditAnywhere)
	FString Name = "jack";
};
```

会在 ue4 实例中显示这些属性

只要完成上述操作，即可在编辑器中编辑该值。还有更多方法可以控制编辑该值的方式和位置。方法是将更多信息传递到 `UPROPERTY` 说明符。例如，如果想要IsDead和Health属性出现在包含相关属性的某个部分中，可以使用分类功能。具体请参见下面的属性声明

```cpp
UPROPERTY(EditAnywhere, Category="Damage")
int32 killsNum = 7;
UPROPERTY(EditInstanceOnly, Category = "Damage")
float Health = 34.45324f;
UPROPERTY(EditAnywhere, Category = "Damage")
bool IsDead = false;
```

创建蓝图类（左蓝图默认，右实例对象）

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220819072220.png)


```cpp
UE_LOG(LogTemp, Warning, TEXT("Name: %s"), *GetName());   // GetName() 自带获取 Actor 名称
UE_LOG(LogTemp, Warning, TEXT("weaponsNum num: %d, kills num: %i"), WeaponsNum, killsNum);
UE_LOG(LogTemp, Warning, TEXT("Health :%f"), Health);
UE_LOG(LogTemp, Warning, TEXT("IsDead : %d, HasWeapon : %d"), IsDead, static_cast<int>(HasWeapon));
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220819073503.png)

在world中的两个对象，都有自己的对应的变量集


### 组件 F 变换类型（TransForm）

头文件：`#include "Components/StaticMeshComponent.h"`


```cpp
// Actor.h
#include "Components/StaticMeshComponent.h"

UCLASS()
class CPP_01_API AMyActor : public AActor 
{
public:	
	UPROPERTY(VisibleAnywhere)    
	UStaticMeshComponent* BaseMesh;   // 静态网格体组件的指针
}


// Actor.cpp
AMyActor::AMyActor()
{
	// 创建组件   两个参数：组件名称
	BaseMesh = CreateDefaultSubobject<UStaticMeshComponent>("BashMesh");
	SetRootComponent(BaseMesh);   // 将根组件指向 Actor，传递指向静态网格体组件的指针
}
```

可以在细节中看到继承 BashMesh 的属性

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220819100718.png)

创建两个静态网个体，并设置形状，通过 TranForm 对象获取详细信息

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220819104421.png)

```cpp
void AMyActor::BeginPlay()
{
	Super::BeginPlay();

	// 访问变换
	FTransform Transform = GetActorTransform();   // 获取 actor 的 Tranform 对象
	FVector Location = Transform.GetLocation();   // 获取 3D 空间位置（平移）
	FRotator Rotator = Transform.Rotator();       // 获取 3D 空间的旋转角度
	FVector Scale = Transform.GetScale3D();       // 获取 3D 空间的比例（缩放）
	
	UE_LOG(LogTemp, Warning, TEXT("Name: %s"), *GetName());
	UE_LOG(LogMyLog, Warning, TEXT("Tranform %s"), *Transform.ToString());
	UE_LOG(LogMyLog, Warning, TEXT("Location %s"), *Location.ToString());
	UE_LOG(LogMyLog, Warning, TEXT("Rotator %s"), *Rotator.ToString());
	UE_LOG(LogMyLog, Warning, TEXT("Scale %s"), *Scale.ToString());
	// 打印更详细的信息
	UE_LOG(LogMyLog, Error, TEXT("Human tranform %s"), *Transform.ToHumanReadableString());


	//printTypes();
	//printStringTypes();
}
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220819104222.png)


（沿正弦）移动

```cpp
// h
protected:
	UPROPERTY(EditAnywhere, Category = "Movement") 
	float Amplitude = 50.0f;    // 振幅
	UPROPERTY(EditAnywhere, Category = "Movement")
	float Frequency = 2.0f;     // 频率
private:
	FVector InitialLocation;



// cpp
void AMyActor::Tick(float DeltaTime)   // 每一帧都调用
{
	Super::Tick(DeltaTime);

	//z = z0 + amplitude * sin(freq * t);   更改 z 坐标实现移动 z = z0 + 振幅 * sin( 频率 * t )
	FVector CurrentLocation = GetActorLocation();   // 获取当前Actor也就是RootComponent根节点的Location坐标位置
	float time = GetWorld()->GetRealTimeSeconds();  // 世界的
	CurrentLocation.Z = InitialLocation.Z + Amplitude * FMath::Sin(Frequency * time);  // 更新 Z 坐标
	SetActorLocation(CurrentLocation);    // 设置当前Actor也就是RootComponent根节点的Location坐标位置
}
```


### 宏：USTRUCT、UENUM

#### 枚举

`UENUM`根据枚举类型来决定是否移动

```cpp
// h

// 枚举类   用于判断运动类型  
UENUM(BlueprintType)   // 参数代表在蓝图编辑器可以使用，否则只能在 C++ 中进行调节
enum class EMovementType : uint8 // uint8 类型别名意味着枚举的最大元素可以为 255{   
   Sin,      // 沿着正弦曲线运动  
   Static    // 此值可以让 actor 处于静止状态  
};

class RIDER_TEST_API AMyActor : public AActor  
{
protected:
	UPROPERTY(EditAnywhere, Category = "Movement")
	EMovementType MoveType = EMovementType::Static;  // 枚举类型，默认 Static
}

// cpp
void AMyActor::Tick(float DeltaTime)  
{  
   Super::Tick(DeltaTime);  
  
   // 根据枚举类型进行切换  
   switch (MoveType)  
   {  
      case EMovementType::Sin:  
         {  
			//z = z0 + amplitude * sin(freq * t);   
			// 更改 z 坐标实现移动 z = z0 + 振幅 * sin( 频率 * t )            
			// 获取当前Actor也就是RootComponent根节点的Location坐标位置  
	        FVector CurrentLocation = GetActorLocation();   
            float time = GetWorld()->GetRealTimeSeconds();  
            // 更新 Z 坐标  
            CurrentLocation.Z = InitialLocation.Z + Amplitude * FMath::Sin(Frequency * time);  
            // 设置当前Actor也就是RootComponent根节点的Location坐标位置
            SetActorLocation(CurrentLocation);      
         }  
         break;  
      case EMovementType::Static: break;  // 为空，不会变化  
      default: break;  
   }  
}
```

可以直接在详细中修改

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220820103633.png)

UMETA()可以用来修饰变量,效果类似UPROPERTY(),下面两种效果是一样的

```cpp
int A UMETA(DisplayName="优秀");

UPROPERTY(DisplayName="优秀")
int A; 
```

C++无法直接定义中文枚举类型，所以需要使用 `UMETA` 宏

枚举名称后面的":uint8" 是为了限制枚举类型占用的内存数量,UE要求如果想要使枚举类型在蓝图中使用,必须要限制所占内存为一个uint8类型,也就是8bit,1字节


#### 结构体

添加了`USTRCUT()`宏，需要在`struct`中假如`GENERATED_BODY()`才能正常使用，如果不需要在蓝图中显示，只在c++中使用，那么 `USTRCUT()` 和 `GENERATED_BODY()` 都不用添加

在UE中的结构体名必须以 F 开头加上结构体名

如果需要存放多个信息，例如：用户的玩家信息等，可以创建结构体数组，将所有的结构体实例对象存放到结构体数组中

将上述信息放入结构体中
```cpp
// 结构体  
USTRUCT(BlueprintType)   // 在蓝图中可编辑  
struct FGeometryData {    // F 开头  
	GENERATED_USTRUCT_BODY();
	UPROPERTY(EditAnywhere, Category = "Movement")     // 可以不需要
	float Amplitude = 50.0f;  
	UPROPERTY(EditAnywhere, Category = "Movement")  
	float Frequency = 2.0f;  
	UPROPERTY(EditAnywhere, Category = "Movement")  
	EMovementType MoveType = EMovementType::Static;  // 枚举类型，默认 Static
};


protected:
	UPROPERTY(EditAnywhere, Category = "Geomentry Data")  
	FGeometryData GeometryData;   // 实例
```

在 ue4 详细面板可以看到相关的属性和初始值
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220820211216.png)



### 材质

在材质编译器中，按下 `1/2/3/4 + click` 按下数字代表分量的向量的数量，最多 4 个 

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220820230127.png)

通过右键节点，可以将节点转换为参数，然后再资产管理器中生成改节点的实例，再实例中可以改变该参数

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220820230922.png)

使用相同材质，但是圆锥是使用实例，再实例中更改 Color 节点的属性值

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220820231011.png)


在蓝图基础上，通过c++可以很快速的更改`Color`参数

在c++中使用基本组件对象`CreateDefaultSubobject<UStaticMeshComponent>("BashMesh");` 

```cpp
// 参数 材料的索引   将创建一个实例材料对象，将其设置为组件，并返回给 point
UMaterialInstanceDynamic* DynMaterial =  BaseMesh->CreateAndSetMaterialInstanceDynamic(0);  
if ( DynMaterial ) {  
   // 设置名称为 Color 的结构 FLinearColor 的颜色  
   DynMaterial->SetVectorParameterValue("Color", FLinearColor::Yellow);       
}
```

将参数添加到结构体中，方便在ue4中修改，同时将实现封装到成员函数中

```cpp
void AMyActor::SetColor(FLinearColor Color) {  
   // 参数 材料的索引   将创建一个实例材料对象，将其设置为组件，并返回给 point   
   UMaterialInstanceDynamic* DynMaterial = BaseMesh->CreateAndSetMaterialInstanceDynamic(0);  
   if ( DynMaterial ) {  
      // 设置名称为 Color 的结构 FLinearColor 的颜色  
      DynMaterial->SetVectorParameterValue("Color", Color);         
	}  
}
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220821075744.png)

### 计时器

在 GeometryData 结构中，新加 float 参数，用于设定计时器频率（间隔，单位 s）

 `FTimerHandle` 描述符创建的变量，可以访问计时器

```cpp
private:  
   FTimerHandle TimerHandle;   // 计时器
```

计时器初始化

```cpp
// 计时器管理器对象的 SetTime 函数初始化 （计时器句柄引用, this指向, 回调函数, 频率, 循环）  
GetWorldTimerManager().SetTimer(TimerHandle, this, &AMyActor::OnTimerFired, GeometryData.TimerRate, true);
```

回调
```cpp
void AMyActor::OnTimerFired() {  
	// 生成随机颜色  
	const FLinearColor NewColor = FLinearColor::MakeRandomColor();     
	SetColor(NewColor);  
}
```

为了在一定次数后停止计时器的调用，可以在类中设定两个成员变量

```cpp
private:
	const int32 MaxTimerCount = 5;   // 最大运行次数  
	int32 TimerCount = 0;    // 当前运行次数
```

```cpp
void AMyActor::OnTimerFired()  
{  
   if (TimerCount <= MaxTimerCount)   // ++TimerCount <= MaxTimerCount 效果相同 
   {  
      // 生成随机颜色  
      const FLinearColor NewColor = FLinearColor::MakeRandomColor();  
      SetColor(NewColor);  
      TimerCount++;  
   }  
   else  
   {  
      // 停止计时器  
      GetWorldTimerManager().ClearTimer(TimerHandle);  
   }  
}
```


### 动态 Actor 生成

```cpp  
// h
#include "MyActor.h"
protected:  
   // Called when the game starts or when spawned  
   virtual void BeginPlay() override;  
  
   // TSubclassOf 指向类的指针  
   UPROPERTY(EditAnywhere)  
   TSubclassOf<AMyActor> MyClass;  // 相比标准类，此模板只会显示指定类以及它的蓝图继承
  
   UPROPERTY(EditAnywhere)  
   UClass* Class;    // ue4 标准的创建类，显示所有，包括自己创建的
   UPROPERTY(EditAnywhere)  
   AMyActor* MyObject;   // 指定类的对象



// cpp
// 头文件包含  World
#include "Engine/World.h"
// Called when the game starts or when spawned  
void AGenmetryHubActor::BeginPlay()  
{  
	Super::BeginPlay();  
  
	// 全局游戏世界对象的指针  
	UWorld* World = GetWorld();  
	if (World)  
	{  
		// 成员函数 SpawnActor 生成 actor 返回该对象 
		AMyActor* My = World->SpawnActor<AMyActor>(MyClass);
		// 参数：生成的 actor 类, 位置, 旋转, 结构体（指定其他生成设置）   
	}  
}
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220821110537.png)

将My Class 设置为蓝图，更改蓝图的信息，将会Genmetry 实例的位置动态生成此 蓝图

随机生成十个，并且设置结构体的参数（在MyActor中公共成员函数 SetGeometryData 设置）
```cpp
// 全局游戏世界对象的指针  
UWorld* World = GetWorld();  
if (World)  
{  
   for (int32 i = 0; i < 10; i++)  
   {  
      // 变换  
      const FTransform MyTransform = FTransform(FRotator::ZeroRotator, FVector(0.0f, 300.0f * i, 300.0f));  
      AMyActor* My = World->SpawnActor<AMyActor>(MyClass, MyTransform);  
      FGeometryData Data;  
      Data.MoveType = FMath::RandBool() ? EMovementType::Sin : EMovementType::Static;  
      My->SetGeometryData(Data);  
   }  
}
```

在创建对象时，会立即调用 `MyActor` 构造函数然后调用 `BeginPlay` 成员函数，由于此时初始化颜色，所以在动态创建中设置颜色并不会更改颜色，此时可以使用 `SpawnActorDeferred` 创建，不会调用`BeginPlay` 成员函数需要手动调用

```cpp
AMyActor* My = World->SpawnActorDeferred<AMyActor>(MyClass, MyTransform);
Data.DefaultColor = FLinearColor::MakeRandomColor();
My->FinishSpawning(MyTransform);   // 位置参数可以改变，会覆盖之前的位置参数(必须传参)
```

在上述的基础上，可以将需要的参数放入结构体中，然后创建存放结构体实对象的数组，就可以通过添加数组内容实现在ue4中控制添加个数以及参数

```cpp
// h
USTRUCT(BlueprintType)  
struct FGeometryPayload  
{  
   GENERATED_USTRUCT_BODY()  
   // TSubclassOf 指向类的指针  
   UPROPERTY(EditAnywhere)  
   TSubclassOf<AMyActor> MyClass;  
   UPROPERTY(EditAnywhere)  
   FGeometryData Data;  
   UPROPERTY(EditAnywhere)  
   FTransform MyTransform;  
};

protected:
	UPROPERTY(EditAnywhere, Category="Array for Object")  
	TArray<FGeometryPayload> GeometryPayloads;

// cpp
UWorld* World = GetWorld();  
for (const FGeometryPayload Payload : GeometryPayloads)  
{  
   AMyActor* My = World->SpawnActorDeferred<AMyActor>(Payload.MyClass, Payload.MyTransform);  
   if (My)  
   {  
      My->SetGeometryData(Payload.Data);  
      My->FinishSpawning(Payload.MyTransform);  
   }  
}
```

在详细中可以自定义添加多个对象，以及对应的属性

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220821122428.png)


### 代表宏：UFUNCTION

`UPROPERTY` 的说明符 `BlueprintReadWrite` 允许访问蓝图图上的属性，`UFUNCTION` 的说明符 `BlueprintCallable` 允许在蓝图上创建该函数节点

```cpp
// MyActor.h
USTRUCT(BlueprintType)  
struct FGeometryData  
{  
   GENERATED_USTRUCT_BODY()  
  
   UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Damage")  
   float Amplitude = 50.0f;  
   UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Damage")  
   float Frequency = 2.0f;  
   UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Damage")  
   EMovementType MoveType = EMovementType::Static;  
   UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Damage")  
   FLinearColor DefaultColor = FLinearColor::Black;  
   UPROPERTY(EditAnywhere, Category = "Movement")   
   float TimerRate = 3.0f; // 计时器频率  
};

public:
	UFUNCTION(BlueprintCallable)  
	FGeometryData GetGeometryData() const;
protected:
	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Geomentry Data")  
	FGeometryData GeometryData;
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220821125813.png)

可以选择右键分割，就会直接在Return 中分开，不需要额外的中断
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220821130118.png)

**委托**

委托使可以储存对具有任意类的特定签名的方法的引用，并在必要时调用此方法

在之前创建计时器时使用了委托，将计时器传递给管理器 `OnTimerFired` 函数，每次触发计时器时，计时器管理器都会调用我们的函数

```cpp
GetWorldTimerManager().SetTimer(TimerHandle, this, &AMyActor::OnTimerFired, GeometryData.TimerRate, true);
```

使用宏 `DECLARE_DELEGATE` 声明委托，相比其他的 `DECLARE_ ...` 声明，此声明只带一个参数：可以分配给委托的任意名称

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220821194443.png)

如果想在蓝图中可以使用简单的委托宏 `DECLARE_DYNAMIC_DELEGATE`，更多的就需要用到其他的

声明两个可以在蓝图中编辑的委托的函数定义：

```cpp
#define DECLARE_DYNAMIC_MULTICAST_DELEGATE_TwoParams( 
	DelegateName,   // 代表  必须 F 开头
	Param1Type,     // 第一个参数类型
	Param1Name,     // 第一个参数
	Param2Type,     // 第二个参数类型
	Param2Name      // 第二个参数
)
```

代理（委托）大致流程：

* **触发点步骤：**
	-   1.声明代理，定义参数列表【可以简单理解为构建一个代理类】
	-   2.声明代理对象【理解为实例化一个类】
	-   3.在需要的位置触发代理（执行或者广播），需要传入参数【相当于发送消息】
* **执行点步骤：**
	-   4.在其他类中需要接收消息位置，绑定实际执行函数【接收消息，并且根据消息内容执行绑定的操作】
	-   5.实现具体的执行函数内容【被触发后，实际执行的内容】

还可以分为三个较为官方的步骤：
-   1.声明代理
-   2.绑定函数到代理
-   3.代理调用

示例：

1、声明两个代理，`Actor` 材质颜色发生变化时，将调用第一个代理。当颜色更改计时器完全结束时，将调用第二个委托

```cpp
// h
DECLARE_DYNAMIC_MULTICAST_DELEGATE_TwoParams(  
   FOnColorChanged,  
   const FLinearColor&,  
   Color,  
   const FString&,  
   Name  
);  
// 代表名称，指向参与者的指针
DECLARE_MULTICAST_DELEGATE_OneParam(FOnTimerFinished, AActor*);
```

2、声明代理实例

```cpp
// h
public:
	UPROPERTY(BlueprintAssignable)  
	FOnColorChanged OnColorChanged;  
	  
	FOnTimerFinished OnTimerFinished;
```

3、在需要的位置触发代理

```cpp
void AMyActor::OnTimerFired()  
{  
   if (++TimerCount <= MaxTimerCount)  
   {  
      // 生成随机颜色  
      const FLinearColor NewColor = FLinearColor::MakeRandomColor();  
      SetColor(NewColor);  
      OnColorChanged.Broadcast(NewColor, GetName());  
   }  
   else  
   {  
      // 停止计时器  
      GetWorldTimerManager().ClearTimer(TimerHandle);  
      OnTimerFinished.Broadcast(this);  
   }  
}
```


4、绑定事件

**在蓝图中绑定**

通过蓝图

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220821205550.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220821205649.png)

设置在屏幕上输出信息

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220821212254.png)

在C++中绑定

添加到动态委托的函数必须使用宏 `UFUNCTION` 标记，此时宏与是否在蓝图显示无关，仅与引擎内存模型有关 

```cpp
// h
// 对应相应委托的函数
private:
	UFUNCTION(BlueprintAssignable)   
	void OnColorChanged(const FLinearColor& Color, const FString& Name);  
	
	void OnTimerFinised(AActor* Actor);

// cpp
void AGenmetryHubActor::OnColorChanged(const FLinearColor& Color, const FString& Name)  
{  
   UE_LOG(LogMyLog, Warning, TEXT("Name: %s \t Color: %s"), *Name, *Color.ToString());  
}
  
void AGenmetryHubActor::OnTimerFinised(AActor* Actor)  
{  
   if (!Actor) return;
   UE_LOG(LogMyLog, Error, TEXT("Timer Finised: %s"), Actor->GetName());  
}

void AGenmetryHubActor::DoActorSpawn()  
{  
   // 全局游戏世界对象的指针  
   UWorld* World = GetWorld();  
   for (const FGeometryPayload Payload : GeometryPayloads)  
   {  
      if (!World) return;  
      AMyActor* My = World->SpawnActorDeferred<AMyActor>(Payload.MyClass, Payload.MyTransform);  
      if (!My) return;  
      My->SetGeometryData(Payload.Data);  
      My->OnColorChanged.AddDynamic(this, &AGenmetryHubActor::OnColorChanged);  // 动态绑定
      My->OnTimerFinished.AddUObject(this, &AGenmetryHubActor::OnTimerFinised);
      My->FinishSpawning(Payload.MyTransform);  
   }  
}
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220821224955.png)

在上面基础上，对添加新 actor 中添加进行销毁功能

```cpp
// cpp
void AGenmetryHubActor::OnTimerFinised(AActor* Actor)  
{  
   if (!Actor) return;  
   UE_LOG(LogMyLog, Error, TEXT("Timer Finised: %s"), *Actor->GetName());  
   AMyActor* My = Cast<AMyActor>(Actor);  
   if (!My) return;  
   UE_LOG(LogMyLog, Display, TEXT("Cast is success, Amplitude %f"), My->GetGeometryData().Amplitude);  
  
   // 销毁  
   My->Destroy();   // 直接销毁
   // My->SetLifeSpan(2.0f);  // 延迟销毁 单位 s
}


// MyActor.h
protected:
	// 自带的 EndPlay 成员方法
	virtual void EndPlay(const EEndPlayReason::Type EndPlayReason) override;

void AMyActor::EndPlay(const EEndPlayReason::Type EndPlayReason)  
{  
   UE_LOG(LogMyLog, Warning, TEXT("Name: %s is dead"), *GetName());  
   Super::EndPlay(EndPlayReason);  
}
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220821225906.png)

### ue 主要类

在ue4世界设置中，可以直接配置的引擎类

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220822001002.png)

新建蓝图，可以看到每个类都哟不同的作用

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220822003526.png)

#### APawn 类

每个关卡都有一个 `APlayerController` 可以让输入设备控制游戏。

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220822004043.png)

##### 键盘输入

在创建项目时，虚幻引擎会自动创建一个 `项目名称 + GameModeBase` 的游戏模式类，在项目根目录中也可在对应 `cpp` 文件和 `h` 文件中修改相关属性

1、新建 `APawn` 类 `SandboxPawn` ，并在游戏模式中设置为默认的 `spawn` 类 

```cpp
#include "SandboxPawn.h"
// 默认游戏模式类 构造函数
Arider_testGameModeBase::Arider_testGameModeBase()  
{  
   // 属性会根据游戏模式创建 pawn 对象 
   DefaultPawnClass = ASandboxPawn::StaticClass();  
}
```

可以看到，世界场景设置的默认 pawn 类变为了自定义的

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220822022552.png)

在基类的构造函数中初始化了所有的类的默认

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220822023037.png)

2、使用轴映射完成前后左右的移动

在项目设置中找到输入，可以看到一些输入绑定

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220822060924.png)

对应的项目目录中的 `Config` 目录下 `ini` 文件（.ini 文件是Initialization File的缩写，即初始化文件 ，是windows的系统配置文件所采用的存储格式。）

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220822061104.png)

**动作映射与轴映射**

动作映射处理的是离散的值。

轴映射处理的是连续的输入。比如，在游戏中按下W键，游戏角色会一直前进，在这个过程中w是一直被按下进行连续的输入，角色持续前进；而跳跃等动作，按一次空格键，游戏角色跳跃一次，是处理离散事件的输入，应该使用动作映射完成。

前后左右的移动

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220822070931.png)

**在 C++ 中编辑**

```cpp
// h
public:  
   // 组件，负责变换  
   UPROPERTY(VisibleAnywhere)  
   USceneComponent* SceneComponent;  
   // 速度  
   UPROPERTY(EditAnywhere)  
   float Velocity = 300.0f;
private:  
   // 记录每一帧的变换
   FVector VelocityVector = FVector::ZeroVector;
   // 前后
   void MoveForward(float Amount);  
   // 左右
   void MoveRight(float Amount);


// cpp
#include "Components/InputComponent.h"

ASandboxPawn::ASandboxPawn()  
{  
   // 使用 root 权限注册根组件  
   SceneComponent = CreateDefaultSubobject<USceneComponent>("SceneComponent");  
   SetRootComponent(SceneComponent);  
}

void ASandboxPawn::Tick(float DeltaTime)  
{  
   // 设置位置  
   if (!VelocityVector.IsZero())  
   {  
      const FVector NewLocation = GetActorLocation() + Velocity * DeltaTime * VelocityVector;  
      SetActorLocation(NewLocation);  
      VelocityVector = FVector::ZeroVector;  // 重置
   }  
}

void ASandboxPawn::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)  
{    
	if (PlayerInputComponent)  
	{  
	   // 绑定轴映射  
	   PlayerInputComponent->BindAxis("MoveForward", this, &ASandboxPawn::MoveForward);  
	   PlayerInputComponent->BindAxis("MoveRight", this, &ASandboxPawn::MoveRight);  
	}
}

void ASandboxPawn::MoveForward(float Amount)  
{  
   UE_LOG(LogSandboxPawn, Warning, TEXT("Move Forward %f"), Amount);  
   VelocityVector.X = Amount;
}  
  
void ASandboxPawn::MoveRight(float Amount)  
{  
   UE_LOG(LogSandboxPawn, Warning, TEXT("Move Right %f"), Amount);  
   VelocityVector.Y = Amount;
}
```

#### APlayerController 类

```cpp
// h
public:
   // 组件，静态网格体  
   UPROPERTY(VisibleAnywhere)  
   UStaticMeshComponent* StaticMeshComponent;  
   // 组件，负责摄像机视口的设置  
   UPROPERTY(VisibleAnywhere)  
   UCameraComponent* CameraComponent;

// cpp
ASandboxPawn::ASandboxPawn()  
{  
   // 附加到根组件上  
   StaticMeshComponent = CreateDefaultSubobject<UStaticMeshComponent>("StaticMeshComponent");  
   StaticMeshComponent->SetupAttachment(GetRootComponent());  
   // 可以附加别的组件上，如静态网格体，按需求更改
   CameraComponent = CreateDefaultSubobject<UCameraComponent>("CameraComponent");  
   CameraComponent->SetupAttachment(GetRootComponent());  
}
```

创建 `SandboxPawn` 的蓝图类 `BP_SandboxPawn`，即可在蓝图比机器中查看到如加上去的摄影机，以及相关参数

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220822073740.png)

多个 `pawn` 之间切换

```cpp
// SandboxPawn.h
public:
	virtual void PossessedBy(AController* NewController) override;  
	virtual void UnPossessed() override;


// SandboxPawn.cpp
void ASandboxPawn::Tick(float DeltaTime)  
{  
   Super::Tick(DeltaTime);  
  
   // 设置位置  
   if (!VelocityVector.IsZero())  
   {  
      const FVector NewLocation = GetActorLocation() + Velocity * DeltaTime * VelocityVector;  
      SetActorLocation(NewLocation);  
      VelocityVector = FVector::ZeroVector;  // 重置  
   }  
}

void ASandboxPawn::PossessedBy(AController* NewController)  
{  
   Super::PossessedBy(NewController);  
  
   UE_LOG(LogSandboxPawn, Display, TEXT("%s Possessed %s"), *GetName(), *NewController->GetName());  
   }  
  
void ASandboxPawn::UnPossessed()  
{  
   Super::UnPossessed();  
  
   UE_LOG(LogSandboxPawn, Display, TEXT("%s UnPossessed"), *GetName());  
}

// SandboxPlayerController.h
protected:  
   virtual void SetupInputComponent() override;  
   virtual void BeginPlay() override;  
  
private:  
   UPROPERTY()  
   TArray<AActor*> Pawns;    // 存储 Pawns 对象
  
   int32 CurrentPawnIndex = 0;   // 切换的下标
   void ChangePawn();   // 切换


// SandboxPlayerController.cpp
#include "SandboxPawn.h"  
  
DEFINE_LOG_CATEGORY_STATIC(LogSandboxPlayer, All, All);  
  
void ASandboxPlayerController::SetupInputComponent()  
{  
   Super::SetupInputComponent();  
  
   InputComponent->BindAction("ChangePawn", IE_Pressed, this, &ASandboxPlayerController::ChangePawn);  
}  
  
void ASandboxPlayerController::BeginPlay()  
{  
   Super::BeginPlay();  
  
  
   UGameplayStatics::GetAllActorsOfClass(GetWorld(), ASandboxPawn::StaticClass(), Pawns);  
}  
  
void ASandboxPlayerController::ChangePawn()  
{  
   if (Pawns.Num() <= 1) return;  

   ASandboxPawn* CurrentPawn = Cast<ASandboxPawn>(Pawns[CurrentPawnIndex]);  
   CurrentPawnIndex = (CurrentPawnIndex + 1) % Pawns.Num();  
   if (!CurrentPawn) return;  
   UE_LOG(LogSandboxPlayer, Display, TEXT("Change player Pawn"));  
   Possess(CurrentPawn);  
}
```

### 模块、目标、建造工具

#### 模块

#### 目标

#### 建造工具




### 垃圾收集器

定期清理内存的机制