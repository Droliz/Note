---
title: IDEA借助MapReduce完成简单的词频统计
zhihu-tags: Hadoop, java, MapReduce, 分布式
zhihu-url: https://zhuanlan.zhihu.com/p/559962938
---

# IDEA借助MapReduce完成简单的词频统计

## 准备

* 1、准备本地 maven 并配置好
* 2、准备分布式环境（hadoop）
* 3、准备IDEA

## maven工程

### 创建工程

在IDEA中配置maven

### pom.xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<project xmlns="http://maven.apache.org/POM/4.0.0"  
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
    <modelVersion>4.0.0</modelVersion>  
  
    <groupId>org.example</groupId>  
    <artifactId>MR_02</artifactId>  
    <version>1.0-SNAPSHOT</version>  
  
    <properties>  
        <maven.compiler.source>8</maven.compiler.source>  
        <maven.compiler.target>8</maven.compiler.target>  
    </properties>  
  
    <dependencies>  
        <dependency>  
            <groupId>org.apache.hadoop</groupId>  
            <artifactId>hadoop-hdfs</artifactId>  
            <version>2.9.2</version>  
        </dependency>  
        <dependency>  
            <groupId>org.apache.hadoop</groupId>  
            <artifactId>hadoop-common</artifactId>  
            <version>2.9.2</version>  
        </dependency>  
        <dependency>  
            <groupId>org.apache.hadoop</groupId>  
            <artifactId>hadoop-client</artifactId>  
            <version>2.9.2</version>  
        </dependency>  
    </dependencies>  
      
</project>
```

### 类 WordCount

```java
import java.io.IOException;  
import org.apache.hadoop.conf.Configuration;  
import org.apache.hadoop.fs.FileSystem;  
import org.apache.hadoop.fs.Path;  
import org.apache.hadoop.io.IntWritable;  
import org.apache.hadoop.io.LongWritable;  
import org.apache.hadoop.io.Text;  
import org.apache.hadoop.mapreduce.Job;  
import org.apache.hadoop.mapreduce.Mapper;  
import org.apache.hadoop.mapreduce.Reducer;  
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;  
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;  
  
public class WordCount {  

	public static class MapTask extends Mapper<LongWritable,Text,Text,IntWritable>{  
        //重写父类的map方法  
        //每读一行数据，map函数就被执行一次  
        @Override  
        protected void map(LongWritable key, Text value, Context context)  
                throws IOException, InterruptedException {  
            String[] words = value.toString().split(",");  
            //遍历words，写出去  
            for(String word:words) {  
                context.write(new Text(word), new IntWritable(1));  //<hadoop,1>  
            }  
        }  
    }  
  
	public static class ReduceTask extends Reducer<Text,IntWritable, Text,IntWritable>{  
		@Override  
        protected void reduce(Text key, Iterable<IntWritable> values,  
                Context context) throws IOException, InterruptedException {  
            for(IntWritable value:values) {  
                count++;  
            }  
            //写出去  
            context.write(key, new IntWritable(count));  
  
        }  
    }  
    //运行测试，本地环境测试  
    public static void main(String[] args) 
	    throws IOException, ClassNotFoundException, InterruptedException {  
        //告诉系统运行的时候，使用root权限  
        System.setProperty("HADOOP_USER_NAME", "root");  
        Configuration conf = new Configuration();  
        conf.set("fs.defaultFS", "hdfs://master:9000");    // 连接
        //1.MR由Job对象去提交任务  
        Job job = Job.getInstance(conf);  
        //2.告知job提交的信息  
        job.setMapperClass(MapTask.class);  
        job.setReducerClass(ReduceTask.class);  
        job.setJarByClass(WordCount.class);  
        //3.告知MR，输出类型 （只需要设置输出类型）  
        //map的输出  
        job.setMapOutputKeyClass(Text.class);  
        job.setMapOutputValueClass(IntWritable.class);  
        //reduce的输出  
        job.setOutputKeyClass(Text.class);  
        job.setOutputValueClass(IntWritable.class);  
  
        //4.输入输出文件的路径设置  
        String output = "/bigdata/output/";  
        //加一个判断  
        FileSystem fileSystem = FileSystem.get(conf);  
        if(fileSystem.exists(new Path(output))) {  
            fileSystem.delete(new Path(output),true);  
        }  
  
        FileInputFormat.addInputPath(job, new Path("/bigdata/input/wc.txt"));  
        FileOutputFormat.setOutputPath(job, new Path(output));  
  
        //为了测试  
        boolean b = job.waitForCompletion(true);  
        System.out.println(b ? "没问题！！" : "失败");  
    }  
}
```

启动 hadoop

```sh
start-all.sh
```

运行java程序，可以在hadoop对应目录下找到生成的统计文件（读取的文件需要提前准备）


测试文件格式为
```txt
java,python,get,set
java,python,get,set
java,python,get,set
java,python,get,set
java,python,get,set
java,python,get,set
java,python,get,set
java,python,get,set
```

同时，也可以直接打包项目为 jar 包，然后上传到服务器

```sh
hadoop jar NAME.jar CLASS_NAME   // 运行
```

同样的可以在指定位置查看到统计好的文件