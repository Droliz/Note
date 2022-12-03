## 启动服务

先切换到es用户，再启动

```
su es
```

```
master:9200
```

![](../markdown_img/Pasted%20image%2020221109161523.png)

```
master:5601
```

![](../markdown_img/Pasted%20image%2020221109161533.png)
## 准备数据

创建索引
![](../markdown_img/Pasted%20image%2020221109161543.png)
向索引添加数据

```
POST /customer/_doc/1
{
"customCode":"1001",
"customerName":"李冰冰",
"age":20,
"sex":0,
"tel":"18968779002",
"addr" :"云龙区桃园小区5号楼",
"fav":"tiaowu,kandianying"
}
POST /customer/_doc/2
{
"customCode":"1002",
"customerName":"李心怡",
"age":25,
"sex":0,
"tel":"18945778890",
"addr":"云龙区铁路21宿舍135号",
"fav":"change,tiaowu"
}
POST /customer/_doc/3
{
"customCode":"1003",
"customerName":"王思淼",
"age":32,
"sex":0,
"tel":"18912268806",
"addr":"云龙区新元公寓25号"
}

POST /customer/_doc/4
{
"customCode":"1004",
"customerName":"张帆",
"age":28,
"sex":1,
"tel":"15862258897",
"addr":"新城区绿地世纪城广场5号楼",
"fav":"changge,guangjie"
}
POST /customer/_doc/5
{
"customCode":"1005",
"customerName":"王强",
"age":35,
"sex":1,
"tel":"13777685530",
"addr":"新城区万福世家8号楼",
"fav":"kanshu,lvyou,kandianying"
}
POST /customer/_doc/6
{
"customCode":"1006",
"customerName":"张子文",
"age":33,
"sex":0,
"tel":"13788934488",
"addr":"新城区维维紫月台二期1栋"
}

POST /customer/_doc/7
{
"customCode":"1007",
"customerName":"张子恒",
"age":32,
"sex":1,
"tel":"18958329958",
"addr":"新城区万福世家3号楼",
"fav":"hejiu,changge,lvyou"
}
POST /customer/_doc/8
{
"customCode":"1008",
"customerName":"周梓玥",
"age":27,
"sex":0,
"tel":"18955889026",
"addr":"新城区万福世家9号楼",
"fav":"hejiu,lvyou"
}
POST /customer/_doc/9
{
"customCode":"1009",
"customerName":"李明",
"age":28,
"sex":1,
"tel":"18982776890",
"addr":"云龙区桃园小区3号楼"
}

POST /customer/_doc/10
{
"customCode":"1010",
"customerName":"赵敏",
"age":26,
"sex":0,
"tel":"189600688579",
"addr":"云龙区桃园小区1号楼",
"fav":"tiaowu,kanshu"
}
POST /customer/_doc/11
{
"customCode":"1011",
"customerName":"张珊珊",
"age":31,
"sex":0,
"tel":"18968778825",
"addr":"铜山新区万科新都汇2号楼",
"fav":"kandianying,changge"
}
POST /customer/_doc/12
{
"customCode":"1012",
"customerName":"万项",
"age":23,
"sex":1,
"tel":"18968778828",
"addr":"云龙区铁路宿舍1010号"
}
```
![](../markdown_img/Pasted%20image%2020221109161555.png)
创建索引并添加数据

```
PUT /index001/_doc/1
{
"name":"zhangsan"
}
PUT /index002/_doc/1
{
"name":"lisi"
}
```
![](../markdown_img/Pasted%20image%2020221109161610.png)
## 聚集查询

### Value Count

值聚合，主要用于统计文档总数，类似SQL的count函数。

```
GET /customer/_search
{
"size" : 0,
"aggs" : {
"count_code" : {
"value_count": {
"field" : "customCode.keyword"
}
}
}
}
```

![](../markdown_img/Pasted%20image%2020221109161621.png)
### Cardinality

基数聚合，也是用于统计文档的总数，跟Value Count的区别是，基数聚合会去重，不会统计重复的值，类似SQL的count(DISTINCT 字段)用法。

```
GET /customer/_search
{
"size" : 0,
"aggs" : {
"cardinality_code" : {
"cardinality": {
"field" : "customCode.keyword"
}
}
}
}
```

提示：是ES的cardinality基数聚合统计的总数是一个近似值，会有一定的误差，这么做的目的是为了性能，因为在海量的数据中精确统计总数是非常消耗性能的。

![](../markdown_img/Pasted%20image%2020221109161629.png)
### 3.Avg
求平均值。
示例：求顾客年龄的平均值：
```
GET /customer/_search
{
"size" : 0,
"aggs" : {
"avg_age" : {
"avg": {
"field" : "age"
}
}
}
}
```

![](../markdown_img/Pasted%20image%2020221109161641.png)

## Sum

求和计算。
主要用于统计文档中某个字段的总和。
```
GET /customer/_search
{
"size" : 0,
"aggs" : {
"sum_age" : {
"sum": {
"field" : "age"
}
}
}
}
```

![](../markdown_img/Pasted%20image%2020221109161658.png)

### Max
求最大值。
```
GET /customer/_search
{
"size" : 0,
"aggs" : {
"max_age" : {
"max": {
"field" : "age"
}
}
}
}
```

![](../markdown_img/Pasted%20image%2020221109161713.png)
### Min
求最小值。
```
GET /customer/_search
{
"size" : 0,
"aggs" : {
"min_age" : {
"min": {
"field" : "age"
}
}
}
}
```
![](../markdown_img/Pasted%20image%2020221109161725.png)
### Status
status -统计聚集，包括min\max\sum\count\avg五项内容。

```
GET /customer/_search
{
"size" : 0,
"aggs" : {
"status_age" : {
"stats": {
"field" : "age"
}
}
}
}
```
![](../markdown_img/Pasted%20image%2020221109161731.png)
等价SQL：
select count(age),min(age),max(age),avg(age),sum(age) from customer

### 综合案例
统计喜欢唱歌的顾客人数，其中年龄最大为多少，最小为多少

```
GET /customer/_search
{
"size" : 0,
"query": {
"match": {"fav": "changge"}
},
"aggs" : {
"count_code" : {
"value_count": {"field" : "customCode.keyword"}
},
"min_age" : {
"min": {"field" : "age"}
},
"max_age" : {
"max": {"field" : "age"}
}
}
}
```
![](../markdown_img/Pasted%20image%2020221109161747.png)
### Terms聚合
terms聚合的作用跟SQL中group by作用一样，都是根据字段唯一值对数据进行分组（分桶），字段值相等的文档都分到同一个桶内。
```
GET /customer/_search
{
"size":0,
"aggs":{
"terms_sex":{
"terms": {
"field": "sex"
}
}
}
}
```

![](../markdown_img/Pasted%20image%2020221109161759.png)
### Histogram聚合
histogram（直方图）聚合，主要根据数值间隔分组，使用histogram聚合分桶统计结果，通常用在绘制条形图报表。
```
GET /customer/_search
{
"size":0,
"aggs":{
"histogram_age":{
"histogram": {
"field": "age",
"interval": 10
}
}
}
}
```

![](../markdown_img/Pasted%20image%2020221109161811.png)
interval：分桶的间隔值，这里以10为间隔进行分组。

### Date histogram聚合
类似histogram聚合，区别是Date histogram可以很好的处理时间类型字段，主要用于根据时间、日期分桶的场景。
```
GET /customer/_search?size=0
{
"aggs" : {
"ps_date" : {
"date_histogram" : {
"field" : "purchaseDate", 
"calendar_interval" : "month",
"format" : "yyyy-MM-dd"
}
}
}
}
```

![](../markdown_img/Pasted%20image%2020221109161820.png)

calendar_interval分组间隔：month代表每月、支持minute（每分钟）、hour（每小时）、day（每天）、week（每周）、year（每年）；
format: 设置返回结果中桶key的时间格式。

### Range聚合
range聚合，按数值范围分桶。
```
GET /customer/_search
{
"size":0,
"aggs" : {
"age_ranges" : {
"range" : {
"field" : "age",
"ranges" : [
{ "to" : 25 },
{ "from" : 25, "to" : 30 },
{"from" :30 }
]
}
}
}
}
```

![](../markdown_img/Pasted%20image%2020221109161828.png)

"to" : 25 : 意思就是 age <= 25的文档归类到一个桶；
"from" : 20, "to" : 30: 20<price<30的文档归类到一个桶；
"from" : 30 : price>30的文档归类到一个桶。

通过观察发现range分桶，默认key的值不太友好，可以为每一个分桶指定一个有意义的名字。
```
GET /customer/_search
{
"size":0,
"aggs" : {
"age_ranges" : {
"range" : {
"field" : "age",
"keyed": true,
"ranges" : [
{"key" : "cheap", "to" : 25 },
{"key" : "average", "from" : 25, "to" : 30 },
{"key" : "expensive", "from" :30 }
]
}
}
}
}
```

![](../markdown_img/Pasted%20image%2020221109161841.png)

### Date_range聚合
date_range聚合，按日期范围分桶。
也可以指定keyed和key定义返回结果标志，多了一个format用于指定日期格式。
```
GET /customer/_search
{
"size": 0
, "aggs": {
"date_ranges": {
"date_range": {
"field": "birthday",
"ranges": [
{
"from": "1990-01-01",
"to": "1999-12-31"
},
{"from": "2000-01-01",
"to": "now"
}
],
"format": "yyyy-MM-dd"
}
}
}
}
```

![](../markdown_img/Pasted%20image%2020221109161851.png)

### 综合案例
配合query语句，先搜索目标文档，然后使用aggs聚合语句对搜索结果进行统计分析。
```
GET /customer/_search
{
"size": 0, // size=0代表不需要返回query查询结果，仅仅返回aggs统计结果
"query" : { // 设置查询语句，先筛选文档
"match" : {"fav" : "changge"}
},
"aggs" : { // 然后对query搜索的结果，进行统计
"sex_terms" : { // 聚合查询名字
"terms" : {"field" : "sex"},
"aggs": { // 通过嵌套聚合查询，设置桶内指标聚合条件
"avg_age": {
"avg": {"field": "age"}
},
"min_age": {
"min": {"field": "age"}
}
}
}
}
}
```

![](../markdown_img/Pasted%20image%2020221109161900.png)

### ES桶聚合支持两种方式排序：

1、内置排序
内置排序参数：
\_count - 按文档数排序。对 terms 、 histogram 、 date_histogram 有效
\_term - 按词项的字符串值的字母顺序排序。只在 terms 内使用
\_key - 按每个桶的键值数值排序, 仅对 histogram 和 date_histogram 有效
示例1：
```
GET /customer/_search
{
"size":0,
"aggs":{
"terms_sex":{
"terms": {
"field": "sex",
"order":{
"_count": "asc"
}
}
}
}
}
```

![](../markdown_img/Pasted%20image%2020221109161920.png)

示例2：
```
GET /customer/_search
{
"size":0,
"aggs":{
"histogram_age":{
"histogram": {
"field": "age",
"interval": 10,
"order":{
"_key": "asc"
}
}
}
}
}
```

![](../markdown_img/Pasted%20image%2020221109161937.png)
按度量指标排序
通常情况下，我们根据桶聚合分桶后，都会对桶内进行多个维度的指标聚合，所以我们也可以根据桶内指标聚合的结果进行排序。
```
GET /customer/_search
{
"size" : 0,
"aggs" : {
"sex_terms" : {
"terms" : {
"field" : "sex",
"order": {
"avg_age" : "asc"
}
},
"aggs": {
"avg_age": {
"avg": {"field": "age"}
}
}
}
}
}
```

![](../markdown_img/Pasted%20image%2020221109161945.png)

如果分桶的数量太多，可以通过给桶聚合增加一个size参数限制返回桶的数量。
```
GET /customer/_search
{
"size" : 0,
"aggs" : {
"age_terms" : {
"terms" : {
"field" : "age",
"size": 3
}
}
}
}
```

![](../markdown_img/Pasted%20image%2020221109161955.png)