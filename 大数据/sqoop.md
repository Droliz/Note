# sqoop 

## # 使用SQOOP将hive的数据导入到mysql

```sh
bin/sqoop export 
--connect jdbc:mysql://localhost:3306/test  # jdbc 
--username root   # 数据库用户名
--password 123456 # 数据库密码
--table uv_info  # 表
--update-mode allowinsert # 数据抽取 默认 updateonly
--hcatalog-table table # hive 表名称
--hcatalog-database database # hive 数据库名称
--hcatalog-partition-keys dt # hive 分区
--hcatalog-partition-keys '2022-12-30' # 分区值
--fields-terminated-by ',' # 分隔符
--columns 'test'  # 指定导入列，默认全部（如果hive不存在  报错）
--num-mappers 1 # 指定mapper数量
```

或

```sh
bin/sqoop export 
--connect jdbc:mysql://localhost:3306/test  # jdbc 
--username root   # 数据库用户名
--password 123456 # 数据库密码
--table uv_info  # 表
--fields-terminated-by ',' # 分隔符
--export-dir /user/hive/warehouse/uv/ # 指定hive在hdfs上存储的目录
--m 1
```