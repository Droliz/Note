# Flume自定义Source

## 准备

准备jdk，1.8及以上
准备flume：[安装flume](https://zhuanlan.zhihu.com/p/511455862)

## 配置

将master节点上的flume的lib目录下的配置文件导出到java工程中

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220513090446.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220513091025.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220513091306.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220513091658.png)

**java代码**

```java
package flume.plugin;
import java.nio.charset.Charset;
import java.util.HashMap;
import org.apache.flume.Context;
import org.apache.flume.Event;
import org.apache.flume.EventDeliveryException;
import org.apache.flume.PollableSource;
import org.apache.flume.conf.Configurable;
import org.apache.flume.event.EventBuilder;
import org.apache.flume.source.AbstractSource;

public class MyFlumeSource extends AbstractSource implements Configurable, PollableSource {
	public synchronized void start() {
		// TODO Auto-generated method stub
		super.start();
		//启动时自动调用，可以进行初始化
		currentvalue=100;
	}

	public synchronized void stop() {
		// TODO Auto-generated method stub
		super.stop();
		//退出时回收资源
		currentvalue=0;
	}

	private static int currentvalue=0;	
	private long interval;
    public long getBackOffSleepIncrement() {
        return 0;
    }

	public long getMaxBackOffSleepInterval() {
	        return 0;
	    }

	public Status process() throws EventDeliveryException {
	    try {
			int s = currentvalue;
			HashMap header = new HashMap();
			header.put("id", Integer.toString(s));
			Event  e=EventBuilder.withBody("no:"+Integer.toString(s), Charset.forName("UTF-8"), header);
			this.getChannelProcessor().processEvent(e);
			currentvalue++;
			Thread.sleep(interval);
        } catch (InterruptedException e) {
            e.printStackTrace();
    	}
        return Status.READY;
    }
	
	public void configure(Context arg0) {
	    interval =arg0.getLong("interval",Long.valueOf(1000l)).longValue();
	}
}
```

**导出jar**

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220513091825.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220513091840.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220513091936.png)

**将打包好的jar包上传到lib目录下**

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220513092044.png)

编写.conf文件并上传到conf目录下

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220513092356.png)

## 测试

在flume的根目录下启动

```sh
bin/flume-ng agent --conf conf --conf-file conf/mysource.conf --name a1 -Dflume.root.logger=INFO,console
```

就可以看到如下结果

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220513092646.png)