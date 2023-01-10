# Flume 项目实战

## 准备

* jdk安装
* [flume安装](https://zhuanlan.zhihu.com/p/511455862)
* tomcat安装

## java web 项目

### 创建 java web 项目

`Flie -> new -> Dynamic Web Project` 创建 java web 项目

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220518162917.png)

如果在项目创建中没有 `Dynamic Web Project` 可以在 `help -> install new software` 中选择对应版本的 work with 来下载所需的

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220518163126.png)

### 项目结构

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220518163230.png)

其中`log4j-1.2.15.jar`自行下载

#### userreg.java
在包 servlets包中创建 servlet 名为 userreg

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220518163409.png)

**userreg.java**

```java
package servlets;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.apache.log4j.Logger;
/**
 * Servlet implementation class userreg
 */
@WebServlet("/userreg")
public class userreg extends HttpServlet {
	/**
	 * 
	 */
	private static final long serialVersionUID = 1L;
	static Logger logger = Logger.getLogger(userreg.class.getName());
    /**
     * @see HttpServlet#HttpServlet()
     */
    public userreg() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.getWriter().append("Served at: ").append(request.getContextPath());
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		String username=request.getParameter("username");
		System.out.println("注册用户："+username+"注册成功！");
		logger.info("username:"+username+",ok!");
		request.getRequestDispatcher("index.jsp").forward(request, response);
		out.flush();
		out.close();
	}
}
```

**log4j.properties**

其中路径`log4j.appender.logfile.File`可以是相对路径，但相对路径可能在开启服务时不会自动创建rt.log文件，这是由于路径如果是相对路径，是相对当前启动 tomcat 的目录，如果配置了环境变量，不在bin目录下运行，就会导致此问题

```properties
log4j.rootLogger=debug, stdout,logfile

log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.err
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout

log4j.appender.logfile=org.apache.log4j.FileAppender
log4j.appender.logfile.File=/opt/tomcat-8.5.37/logs/rt.log
log4j.appender.logfile.layout=org.apache.log4j.PatternLayout
log4j.appender.logfile.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %l %F %p %m%n
```

**index.jsp**

```js
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="ISO-8859-1">
<title>Insert title here</title>
</head>
<body>

<form action="userreg" method="post">
 用户名称:<input type="text" name="username">
 <p>
<input type="submit" value="注册">
</form><br>

</body>
</html>
```

### 导出

导出为 war 包

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220518163836.png)

## 测试

将war包上传到tomcat的webapps目录下

在flume中添加采集配置

```conf
a1.sources = r1
a1.sinks = k1
a1.channels = c1

#Describe/configure the source
#监听目录,spoolDir指定目录, fileHeader要不要给文件夹前坠名
a1.sources.r1.type = exec
a1.sources.r1.command=tail -F /opt/tomcat-8.5.37/logs/rt.log
a1.sources.r1.fileHeader = true

#Describe the sink
a1.sinks.k1.type=logger

#Use a channel which buffers events in memory
a1.channels.c1.type = memory
a1.channels.c1.capacity = 1000
a1.channels.c1.transactionCapacity = 100

#Bind the source and sink to the channel
a1.sources.r1.channels = c1
a1.sinks.k1.channel = c1
```

先运行 tomcat 再运行 flume

```sh
# 运行tomcat
startup.sh

# 运行flume
bin/flume-ng agent --conf conf --conf-file conf/tomcatlog.conf --name a1 -Dflume.root.logger=INFO,console
```

随后再网页的 8080 端口的 Tomcat 可以看到表单，点击提交可以在 flume 窗口看到消息

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220518164445.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220518162547.png)

在 tomcat 的日志文件中也可以查看到刚刚的post提交

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220518162641.png)