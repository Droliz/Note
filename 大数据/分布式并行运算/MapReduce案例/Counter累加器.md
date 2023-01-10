# Counter累加器

## 准备

自定义计数器用的比较广泛，特别是统计无效数据条数的时候，我们就会用到计数器来记录错误日志的条数。

自定义计数器，统计输入的无效数据:  

数据集   假如一个文件，规范的格式是3个字段，“\t”作为分隔符，其中有2条异常数据，一条数据是只有2个字段，一条数据是有4个字段

```txt
001 5   11
002 0   23
003 6
004 0   21 45
```

## 代码

### map

```java
public static class MapCounter extends Mapper<LongWritable, Text, Text, Text> {  
    public static Counter ct = null;  
    protected void map(LongWritable key,Text value, Context context)  
            throws java.io.IOException, InterruptedException {  
        String[] arr_value = value.toString().split("\t");  
        if (arr_value.length < 3) {  
            ct = context.getCounter("ErrorCounter", "TooShort");  
            ct.increment(1); // 计数器加一  
        } else if (arr_value.length > 3) {  
            ct = context.getCounter("ErrorCounter", "TooLong");  
            ct.increment(1);  
        }  
    }  
}
```

### main

```java
public static void main(String[] args) 
	throws IOException,InterruptedException, ClassNotFoundException{  
	//告诉系统运行的时候，使用root权限  
	System.setProperty("HADOOP_USER_NAME", "root");  
	Configuration conf = new Configuration();  
	conf.set("fs.defaultFS", "hdfs://master:9000");  
	//1.MR由Job对象去提交任务  
	Job job = Job.getInstance(conf);  
	//2.告知job提交的信息  
	job.setMapperClass(MapCounter.class);  
	job.setJarByClass(Counters.class);  
	//3.告知MR，输出类型 （只需要设置输出类型）  
	//map的输出  
	job.setMapOutputKeyClass(SecondSort.class);  
	job.setMapOutputValueClass(Text.class);  
	//4.输入输出文件的路径设置  
	String output = "/bigdata/output/Counters/";  
	//加一个判断  
	FileSystem fileSystem = FileSystem.get(conf);  
	if (fileSystem.exists(new Path(output))) {  
		fileSystem.delete(new Path(output), true);  
	}  
	FileInputFormat.addInputPath(job, new Path("/bigdata/input/Counters/"));  
	FileOutputFormat.setOutputPath(job, new Path(output));  
	//为了测试  
	boolean b = job.waitForCompletion(true);  
	System.out.println(b ? "T" : "F");  
}
```


## 结果

可以在运行的日志中看到

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220908114558.png)