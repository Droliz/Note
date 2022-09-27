### 站点表

记录站点的坐标以及站点名字和名字的坐标

```txt
Name    X    Y    xT    yT
```

* Name：站点名字   string
* X / Y：站点的坐标     
* xT / yT：站点名字的坐标    



### 站点对应关系表

记录两个**相邻**站点之间的对应关系

两两之间只记录一次：如 A -> B    B -> A   只记录一个即可

```txt
Id    From    To    Flag
```

* Id：行号     int
* From：站点一   站点名称  string
* To：站点二     站点名称 string
* Flag：虚线：false  实线：true



### 记录分割站点

地图上有两个黑横线的旁边的点（最近）的信息

```txt
Name    X    Y    xT    yT
```

* Name：站点名字
* X / Y：站点的坐标
* xT / yT：站点名字的坐标
