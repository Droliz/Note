# Datagrip连接phoenix

## 准备工作

* 1、Datagrip
* 2、Phoenix
* 3、JDBC驱动（Phoenix的根目录下的phoenix-xxx-Hbase-2.0-client.jar）

## 连接

### 添加驱动

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220415132618.png)

添加驱动文件

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220415132913.png)

更改类为`org.apache.phoenix.jdbc.PhoenixDriver`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220415132953.png)

设置高级配置

添加用户自定义和jdk路径

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220415133208.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220415133257.png)

驱动程序为刚刚配置的

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220415133327.png)

url为`jdbc:phoenix:ip地址:2181`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220415133441.png)

