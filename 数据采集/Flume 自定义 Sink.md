# Flume 自定义 Sink

## 准备

* jdk
* ecilpse
* windows 开启telnet服务
* [flume的安装配置](https://zhuanlan.zhihu.com/p/511455862)
* flume 的相关配置 jar 包

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220520084234.png)

## java项目

### 项目结构

lib目录下的jar包需要添加到配置中

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220520084919.png)

### java 代码

```java
package flume;

import org.apache.flume.Channel;
import org.apache.flume.Context;
import org.apache.flume.Event;
import org.apache.flume.EventDeliveryException;
import org.apache.flume.Transaction;
import org.apache.flume.conf.Configurable;
import org.apache.flume.sink.AbstractSink;
import java.io.File;
import java.io.FileWriter;
import java.io.BufferedWriter;

public class MySink extends AbstractSink implements Configurable {
    //输出文件的路径和文件名称
	private String url;

	@Override
	public Status process() throws EventDeliveryException {
		Channel ch = getChannel();
		Transaction txn = ch.getTransaction();
		Event event = null;
		txn.begin();
		while (true) {
			event = ch.take();
			if (event != null) {
				break;
			}
		}
		try {
			String msg = new String(event.getBody());
			String content = msg;
			File file = new File(url);
			if (!file.exists())
				file.createNewFile();
			BufferedWriter output = new BufferedWriter(new FileWriter(file, true));
			output.write(content);
			output.write("\r\n");
			output.flush();
			output.close();
			System.out.println("Done");
			txn.commit();
			return Status.READY;
		} catch (Throwable th) {
			txn.rollback();
			if (th instanceof Error) {
				throw (Error) th;
			} else {
				throw new EventDeliveryException(th);
			}
		} finally {
			txn.close();
		}
	}

	@Override
	public void configure(Context arg0) {
// 从配置文件中获得url路径信息（在配置文件中设定自定义Source设定的输出文件的路径和文件名称）
		url = arg0.getString("url");
	}
}
```

### 导出 jar 包

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220520085419.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220520085357.png)

## 配置flume

添加flume的采集配置`mysink.conf`

```conf
#a1是agent的代表。  
a1.sources = r1  
a1.sinks = k1  
a1.channels = c1

a1.sources.r1.type = netcat  
a1.sources.r1.bind = master  
a1.sources.r1.port = 444

a1.sinks.k1.type = flume.MySink  
a1.sinks.k1.url =/home/mysink.txt

a1.channels.c1.type = memory  
a1.channels.c1.capacity = 1000  
a1.channels.c1.transactionCapacity = 100

#Bind the source and sink to the channel  
a1.sources.r1.channels = c1  
a1.sinks.k1.channel = c1
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220520090119.png)

## 测试

上传到 flume 的lib目录下

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220520085705.png)

启动 flume 

```sh
bin/flume-ng agent --conf conf --conf-file conf/mysink.conf --name a1 -Dflume.root.logger=INFO,console
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220520090139.png)

连接 master

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220520090226.png)

发送的消息，flume会调用mysink.jar然后保存在url地址，此次实验地址在home目录下

由于前面在java项目中设置的输出内容是Done所以这里输出Done，可以自行更改

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220520090322.png)

查看输出的文件

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220520090509.png)