# MapReduce

## 介绍

如今各种类型的衍生数据的数据量巨大，完成运算需要大量的时间，只有将**计算分布在成百上千的主机**上，而**分发数据**、**数据处理与运算**、**处理错误**、都需要大量的代码处理  
为了解决此问题，设计一个新的**抽象模型**，**只用来表述**想要执行的简单运算，而其他的计算、容错、数据分布、负载均衡等复杂的细节都封装在库中  
这个工作(实现一个MapReduce框架模型)的主要贡献是通过简单的接口来实现自动的并行化和大规模的分布式计算，通过使用MapReduce模型接口实现在大量普通的PC机上高性能计算  
* 1、第二部分描述基本的编程模型和一些使用案例
* 2、第三部分描述了一个经过裁剪的、适合我们的基于集群的计算环境的MapReduce实现
* 3、第四部分描述为在MapReduce编程模型中一些实用的技巧
* 4、第五部分对于各种不同的任务，测量MapReduce实现的性能
* 5、第六部分揭示了在Google内部如何使用MapReduce作为基础重写索引系统产品，包括其它一些使用MapReduce的经验
* 6、第七部分讨论相关的和未来的工作

### MapReduce优势

* 并行处理
* 数据本地性

## 编程模型

MapReduce编程模型的原理是：利用一个输入**key/value pair集合**来产生一个输出的key/value pair集合。MapReduce库的用户用两个函数表达这个计算：Map和Reduce

用户自定义的Map函数接受一个输入的key/value pair值，然后产生一个中间key/value pair值的集合。MapReduce库把所有**具有相同中间key值**I的中间value值集合在一起后传递给reduce函数

用户自定义的Reduce函数接受一个中间key的值I和**相关的一个value值的集合**。Reduce函数合并这些value值，形成一个较小的value值的集合。一般的，每次Reduce函数调用只产生**0或1**个输出value值。通常我们**通过一个迭代器**把中间value值提供给Reduce函数，这样我们就可以处理无法全部放入内存中的大量的value值的集合

### 示例

```java
计算一个大的文档集合中每个单词出现的次数
// 定义map函数
map(String key, String value):
    // key: 元素名字
    // value: 元素内容
    // 遍历
    for w in value:
        EmitIntermediate(w, '1');

// 定义reduce函数
reduce(String key, Iterator values):
    // key: 单词
    // values: 计数列表
    int result = 0;
    for v in values:
        result += ParseInt(v);
    Emit(AsString(result));
```

### 类型

对于用户自定义的Map和Reduce有相关联的类型：`map(k1, v1) -> list(k1, va) reduce(k2, list(k2)) -> list(v2)`比如，输入的key和value值与输出的key和value值在类型上推导的域不同。此外，中间key和value值与输出key和value值在类型上推导的域相同

### 示例

* 分布式的Grep：Map函数输出匹配某个模式的一行，Reduce函数是一个恒等函数，即把中间数据复制到输出
    * Grep：grep全称是Global Regular Expression Print，表示**全局正则表达式版本**，它的使用权限是**所有用户**，Linux系统中grep命令是一种强大的文本搜索工具

* 计算URL访问频率：Map函数处理日志中web页面请求的记录，然后输出(URL,1)。Reduce函数把相同URL的value值都累加起来，产生(URL,记录总数)结果

* 倒转网络链接图：Map函数在源页面（source）中搜索所有的链接目标（target）并输出为(target,source)。Reduce函数把给定链接目标（target）的链接组合成一个列表，输出(target,list(source))
 
* 倒排索引：Map函数分析每个文档输出一个(词,文档号)的列表，Reduce函数的输入是一个给定词的所有（词，文档号），排序所有的文档号，输出(词,list（文档号）)。所有的输出集合形成一个简单的倒排索引，它以一种简单的算法跟踪词在文档中的位置

* 分布式排序：Map函数从每个记录提取key，输出(key,record)。Reduce函数不改变任何的值。这
个运算依赖**分区机制**和**排序属性**

## 实现

![MapReduce模型的实现](http://www.droliz.cn/markdown_img/mapreduce模型的实现.png)

* map reduce编程模型把数据运算流程分成2个阶段
    * 阶段1：读取原始数据，形成key-value数据(map方法)
    * 阶段2: 将阶段1的key-value数据按照相同key分组聚合(reduce方法)

对于一个包含成千上百的集群，机器故障是常态

### 执行概括

![执行过程](http://www.droliz.cn/markdown_img/实现过程.jpg)

* 用户程序调用Map Reduce库将输入文件分成**多个数据片段（M、R）**（每个大小一般都是16MB-64MB，可通过参数设置），然后再用户程序的**集群中**创建大量程序的副本
* 副本中有一个master程序，负责给其他worker程序分配任务
* 被分配了map任务的worker，再map任务执行完成，文件是在本地进行分区并**存储在本地磁盘上**的，并不回存HDFS中（中间数据，只有当reduce执行完成的才是最终数据），当reduce执行完成就会立即删除
    * HDFS：分布式文件系统
* 由master给reduce分配任务，将处理好的文件追加到该分区的输出文件中，当全部文件处理完成最后的输出结果就会存储在HDFS中，第一块副本存储在本地节点上，其他副本存储在其他的机架节点中（**一般作为另一个Map Reduce的输入**）

### master数据结构
master中存储每一个Map和Reduce任务的状态（空闲、工作中、完成）以及Worker机器（非空闲任务的机器）的标识  
master存储的关于Map任务产生的中间文件的存储区域的大小和位置，当Map任务完成就会将这些信息推送给Reduce

### 容错

* **worker故障**
    
    在一个约定时间内，没有接收到worker返回的消息，标记此worker为失效，被安排的所有任务被全部设置空闲交给其他worker，同样的所有与之相关的reduce也被重置，等待重新调度

* **master失效**
    
### 存储位置

在计算运行环境中，网络带宽是一个相当匮乏的资源。我们通过尽量把输入数据(由GFS管理)存储在集群中机器的本地磁盘上来节省网络带宽。master在调度任务时会优先考虑处理的Map和数据在同一个机器上，以达到减少对网络带宽的消耗

### 任务粒度

在理想情况下M和R片段比集群中的worker大得多，但在现实情况下是有一定的客观限制，master至少会执行O（M + R）次调度，并且在内存中保存O（M * R）个状态（由于R可以直接影响最后需求的数据的文件夹个数，所以优先考虑R）

### 备用任务

在处理一组数据的过程中，如果一个机器速度非常的慢而导致整体的时间超过预期，那么在MapReduce操作即将完成的时候，master调用备用（backup）任务进度来执行剩下的处于处理中的任务

## 单词计数示例

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
    /* 输入数据的类型  数据再mr内部传递的时候需要序列化
     * LongWritable  序列化 （MR框架维护）
     * Text  输入的数据（读到文件中一行）等价于 StringWritable
     *
     * 输出数据的类型   根据业务来确定  Word value（98）
     * Text
     * IntWritable
     */
    public static class MapTask extends Mapper<LongWritable,Text,Text,IntWritable>{
        //重写父类的map方法
        //每读一行数据，map函数就被执行一次
        @Override
        protected void map(LongWritable key, Text value, Context context)
                throws IOException, InterruptedException {
            // TODO Auto-generated method stub
            // key --  value   word -- 1
            //value    hadoop,hadoop,spark,spark
            //结果是：<hadoop,1> <hadoop,1>  <spark,1>  <spark,1>
            String[] words = value.toString().split(",");
            //遍历words，写出去
            for(String word:words)
            {
                context.write(new Text(word), new IntWritable(1));//<hadoop,1>
            }
        }
    }

    //Reduce端  <hadoop,96>
    /* 输入数据的类型  跟MapTask的输出类型一致
     * Text  输入的数据（读到文件中一行）等价于 StringWritable
     * IntWritable
     *
     * 输出数据的类型   根据业务来确定  Word value（98）
     * Text
     * IntWritable
     */
    public static class ReduceTask extends Reducer<Text,IntWritable, Text,IntWritable>{
        /*
         * 重写父类的reduce方法
         * 每替换一个key的时候，reduce方法被调用一次
         */
        @Override
        protected void reduce(Text key, Iterable<IntWritable> values,
                              Context context) throws IOException, InterruptedException {
            // TODO Auto-generated method stub
            //super.reduce(key, values, context);
            //key hadoop
            //values  (1,1,1,1,...)
            int count = 0;
            for(IntWritable value:values) {
                count++;
            }
            //写出去
            context.write(key, new IntWritable(count));

        }
    }
    //运行测试，本地环境测试
    public static void main(String[] args) throws IOException, ClassNotFoundException, InterruptedException {
        // TODO Auto-generated method stub
        //告诉系统运行的时候，使用root权限
        System.setProperty("HADOOP_USER_NAME", "root");
        Configuration conf = new Configuration();
        conf.set("fs.defaultFS", "hdfs://master:9000");
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

可以打包为 jar 在master上运行

```sh
hadoop jar FILE_NAME.jar CLASS_NAME
```



## 流程


Map首先将输出写到环形缓存当中，开始spill过程：  
**job.setPartitionerClass(PartitionClass.class);**  
【按key分区】map阶段最后调用。对key取hash值(或其它处理)，指定进入哪一个reduce

**job.setSortComparatorClass(SortComparator.class);**  
【按key排序】每个分区内，对 键 或 键的部分 进行排序，保证分区内局部有序；

**job.setGroupingComparatorClass(Grouptail.class);**  
【按key分组】构造一个key对应的value迭代器。同一分区中满足同组条件（可以是不同的key）的进入同一个Interator，执行一次reduce方法；


![](../../markdown_img/Pasted%20image%2020220909151548.png)