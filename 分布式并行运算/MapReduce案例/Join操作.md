##  准备

数据格式

两个文件 `item.txt` 和 `order.txt`

```txt
item.txt
1       1000    diannao
2       1001    shouji

order.txt
1001    zhangsan
1002    lisi
1003    wangwu
```


## 分析

在一个mapreduce的流程中，由map到reduce的过程中对数据一共可以进行以下三种操作

**job.setPartitionerClass(PartitionClass.class);**  
【按key分区】map阶段最后调用。对key取[hash](https://so.csdn.net/so/search?q=hash&spm=1001.2101.3001.7020)值(或其它处理)，指定进入哪一个reduce

**job.setSortComparatorClass(SortComparator.class);**  
【按key排序】每个分区内，对 键 或 键的部分 进行排序，保证分区内局部有序；

**job.setGroupingComparatorClass(Grouptail.class);**  
【按key分组】构造一个key对应的value迭代器。同一分区中满足同组条件（可以是不同的key）的进入同一个Interator，执行一次[reduce](https://so.csdn.net/so/search?q=reduce&spm=1001.2101.3001.7020)方法；


此实验中：

两个文件之间的数据可以通过 id 来连接也就是需要根据不同文件来拆分出 id 当作map输出的 key，并且对于不同文件打上为唯一标识，以便后面分区

然后通过 Practitioners 将map的结果发送到相应的reduce。HashPartitioner是mapreduce的默认partitioner。计算方法是
`which reducer=(key.hashCode() & Integer.MAX_VALUE) % numReduceTasks`，得到当前的目的reducer


## 代码

### TextPair类

```java
class TextPair implements WritableComparable<TextPair> {  
    private Text first;  
    private Text second;  
  
    public TextPair() {  
        set(new Text(), new Text());  
    }  
  
    public TextPair(String first, String second) {  
        set(new Text(first), new Text(second));  
    }  
  
    public TextPair(Text first, Text second) {  
        set(first, second);  
    }  
  
    public void set(Text first, Text second) {  
        this.first = first;  
        this.second = second;  
    }  
  
    public Text getFirst() {  
        return first;  
    }  
  
    public Text getSecond() {  
        return second;  
    }  
  
    public void write(DataOutput out) throws IOException {  
        first.write(out);  
        second.write(out);  
    }  
  
    public void readFields(DataInput in) throws IOException {  
        first.readFields(in);  
        second.readFields(in);  
    }  
  
    public int compareTo(TextPair tp) {  
        int cmp = first.compareTo(tp.first);  
        if (cmp != 0) {  
            return cmp;  
        }  
        return second.compareTo(tp.second);  
    }  
  
    public String toString() {  
        return first + "\t" + second;  
    }  
}
```

### map

```java
public static class MapTask extends Mapper<LongWritable, Text, TextPair, Text> {  
    @Override  
    protected void map(LongWritable key, Text value, Context context)  
            throws IOException, InterruptedException {  
        // 获取输入文件的全路径和名称  
        String pathName = ((FileSplit) context.getInputSplit()).getPath().getName();  
        String[] values = value.toString().split("\t");  
  
        if (pathName.contains("item.txt")) {  
            if (values.length < 3) {  
                // data数据格式不规范，字段小于 3 ，抛弃数据  
                return;  
            } else {  
                // 数据格式规范，区分标识为1  
                TextPair tp = new TextPair(new Text(values[1]), new Text("1"));  
                context.write(tp, new Text(values[0] + "\t" + values[2]));  
            }  
        }  
  
        if (pathName.contains("order.txt")) {  
            if (values.length < 2) {  
                // data数据格式不规范，字段小于 2 ，抛弃数据  
                return;  
            } else {  
                // 数据格式规范，区分标识为0  
                TextPair tp = new TextPair(new Text(values[0]), new Text("0"));  
                context.write(tp, new Text(values[1]));  
            }  
        }  
    }  
}
```

### Practitioners

```java
// HashPartitioner是mapreduce的默认partitioner。计算方法是  
// `which reducer=(key.hashCode() & Integer.MAX_VALUE) % numReduceTasks`，得到当前的目的reducer  
public static class Practitioners extends Partitioner<TextPair, Text> {  
    @Override  
    public int getPartition(TextPair key, Text value, int numPartition) {  
        return Math.abs(key.getFirst().hashCode() * 127) % numPartition;  
    }  
}  
```

### Comparators

```java
// 如果连续**（注意，一定连续）**的两条或多条记录满足同组（即compare方法返回0）的条件，那么会把这些key对应的value放在同一个集合中  
public static class Comparators extends WritableComparator {  
    public Comparators() {  
        super(TextPair.class, true);  
    }  
  
    public int compare(WritableComparable a, WritableComparable b) {  
        TextPair t1 = (TextPair) a;  
        TextPair t2 = (TextPair) b;  
        return t1.getFirst().compareTo(t2.getFirst());  
    }  
}
```

### reduce

```java
public static class ReduceTask extends Reducer<TextPair, Text, Text, Text> {  
    @Override  
    protected void reduce(TextPair key, Iterable<Text> values, Context context)  
            throws IOException, InterruptedException {  
        Text First = key.getFirst();  
        String val = values.iterator().next().toString();  
        while (values.iterator().hasNext()) {  
            Text v = new Text(values.iterator().next().toString() + "\t" + val);  
            context.write(First, v);  
        }  
    }  
}
```


### main

```java
public static void main(String[] args)  
        throws IOException, InterruptedException, ClassNotFoundException {  
    //告诉系统运行的时候，使用root权限  
    System.setProperty("HADOOP_USER_NAME", "root");  
    Configuration conf = new Configuration();  
    conf.set("fs.defaultFS", "hdfs://master:9000");  
    //1.MR由Job对象去提交任务  
    Job job = Job.getInstance(conf);  
    //2.告知job提交的信息  
    job.setMapperClass(MapTask.class);  
    job.setPartitionerClass(Practitioners.class);   // 分区  
    job.setGroupingComparatorClass(Comparators.class);   // 对发往reduce的 键值对进行分组操作  
    job.setReducerClass(ReduceTask.class);  
    job.setJarByClass(Join.class);  
    //3.告知MR，输出类型 （只需要设置输出类型）  
    //map的输出  
    job.setMapOutputKeyClass(TextPair.class);  
    job.setMapOutputValueClass(Text.class);  
    //reduce的输出  
    job.setOutputKeyClass(Text.class);  
    job.setOutputValueClass(Text.class);  
    //4.输入输出文件的路径设置  
    String output = "/bigdata/output/Join/";  
    //加一个判断  
    FileSystem fileSystem = FileSystem.get(conf);  
    if (fileSystem.exists(new Path(output))) {  
        fileSystem.delete(new Path(output), true);  
    }  
    FileInputFormat.addInputPath(job, new Path("/bigdata/input/Join/"));  
    FileOutputFormat.setOutputPath(job, new Path(output));  
    //为了测试  
    boolean b = job.waitForCompletion(true);  
    System.out.println(b ? "T" : "F");  
}
```


## 结果

![](../../markdown_img/Pasted%20image%2020220909143139.png)