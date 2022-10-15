---
title: HBase Shell常用命令
注意: 所有的冒号是半角冒号，冒号后面有一个半角空格
zhihu-url: https://zhuanlan.zhihu.com/p/496107262
---

# HBase Shell常用命令

## 启动HBase

`jps`查看目前的进程

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404085428.png)

`/opt/hbase-2.1.0/bin/start-hbase.sh`启动hbase

此时再次查看进程`jps`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404085540.png)

进入hbase shell

```sh
/opt/hbase-2.1.0/bin/hbase shell
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404085659.png)


## help（查看帮助信息）
`help [参数]`
参数可选，可以包含需要查看的

```
help 	# 查看帮助信息
help 'create' 查看create语法
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404090442.png)


### namespace（命名空间）
#### 创建命名空间

`create_namespace '空间名'`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404090849.png)

#### 列出所有命名空间

`list_namespace`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404090942.png)

#### 获取命名空间的描述

`describe_namespace '空间名'`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404091131.png)


#### 列出命名空间下的所有表

`list_namespace_tables '空间名'`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404091318.png)


## ddl命令
### create（创建表）

`create '命名空间名:表名', '列簇名1', '列簇名2', ……`
不指定命名空间表示default表空间
`create '表名', {NAME => '列族名1', VERSIONS => 版本号, TTL => 过期时间, BLOCKCACHE => true}, {NAME => '列族名2', VERSIONS => 版本号, TTL => 过期时间, BLOCKCACHE => true}, ……`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404092335.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404093254.png)

### list（查看所有表）

`list`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404093402.png)

### exists（查看表是否存在）

`exists '命名空间名:表名'`
如果命名空间不写，默认是看default的表

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404093543.png)

### alter（修改表）

`alter '表名', {NAME=>'列簇名1:列名1, 列簇名2:列名2', 参数名=>参数值, ……}`添加
`alter '表名', {NAME=>'列簇名', METHOD=>'delete'}`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404094205.png)

### en/disable（禁用启用表）
`enable '表名'`启用表
`disable '表名'`禁用表

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404094208.png)

### drop（删除表）
删除的表必须处于禁用的状态，启用的表是不能删除的
先禁用表`disab;e '表名'`
删除表`drop '表名'`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404095520.png)

## dml 命令

### put（修改数据）

`put '命名空间名:表名', '行键', '列簇:列名', '值'`

只有一个列时列名可选

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404095831.png)

### scan（全表扫描）

`scan '表名'[, {COLUMN=>'列簇名1:列名1, 列簇名2:列名2', 参数名=>参数值, ……}]`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404101051.png)

### get（获取数据）

`get '表名', '行键'[, {COLUMN=>'列簇名1:列名1, 列簇名2:列名2', 参数名=>参数值, ……}]`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404101102.png)

### delete/deleteall（删除数据）

`delete '表名', '行键', '列族名:列名'`删除一列
`deleteall '表名', '行键'`删除一行

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404101113.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404101122.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404101759.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220404102230.png)