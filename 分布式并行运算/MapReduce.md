```java
import org.apache.hadoop.io.IntWritable;  
import org.apache.hadoop.io.LongWritable;  
import org.apache.hadoop.io.Text;  
import org.apache.hadoop.mapreduce.Job;  
import org.apache.hadoop.mapreduce.Mapper;  
import org.apache.hadoop.mapreduce.Reducer;  
import java.io.IOException;  
  
public class WordCount {  
  
  
    public static class Map extends Mapper<LongWritable, Text, Text, IntWritable> {  
  
        protected void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {  
            String line = value.toString();  
            String[] str = line.split(",");  
            for (String s: str) {  
                context.write(new Text(s), new IntWritable(1));  
            }  
        }  
    }  
  
    public static class Reduce extends Reducer<Text, IntWritable, Text, IntWritable> {  
        protected void reduce(Text key, Iterable<IntWritable> value, Context context) throws IOException, InterruptedException {  
            int count = 0;  
            for (IntWritable val: value) {  
                count++;  
            }  
            context.write(key, new IntWritable(count));  
        }  
    }  
  
  
    public static void main(String[] args) throws IOException {  
        Job job = Job.getInstance();  
        job.setMapperClass(Map.class);  
        job.setReducerClass(Reduce.class);  
        job.setJarByClass(WordCount.class);  
        job.setOutputKeyClass(Text.class);  
        job.setOutputValueClass(IntWritable.class);  
  
          
    }  
}
```