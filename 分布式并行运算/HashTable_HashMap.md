
### 原始实现单词统计
```java
import java.io.*;  
import java.util.*;  
  
public class WorldCount {  
    public static void MapReduce() throws IOException {  
        BufferedReader br = null;  
        BufferedWriter wr = null;  
        HashMap<String,Integer> map = new HashMap<>();  
  
        try {  
            FileInputStream in = new FileInputStream("E:\\words.txt");  
            br = new BufferedReader(new InputStreamReader(in));  
  
            String line = null;  
            while ((line = br.readLine())!=null) {  
                //处理数据  
                String [] words =line.split(",");  
                for (String word:words) {  
                    Integer count = map.getOrDefault(word,0);  
                    count++;  
                    map.put(word,count);  
                }  
            }  
            //排序  
            Set<Map.Entry<String,Integer>> entries = map.entrySet();  
            //转成ArrayList,添加排序规则  
            ArrayList<Map.Entry<String,Integer>> lists = new ArrayList<>(entries);  
            // 可以使用 comparator.comparingInt(Map.Entry::getValue)            
            lists.sort((o1, o2) -> o1.getValue()-o2.getValue());   // lambda 表达式  
  
            //3.显示  
            for (Map.Entry<String,Integer> entry: lists) {  
                System.out.println(entry.getKey()+" -- "+entry.getValue());  
            }  
            //4.写入  
            FileOutputStream out = new FileOutputStream("E:\\newwords.txt");  
            wr = new BufferedWriter(new OutputStreamWriter(out));  
            for (Map.Entry<String,Integer> entry:lists) {  
                wr.write(entry.getKey() + " -- "+entry.getValue()+"\r");  
                wr.flush();  
            }  
        } catch (IOException e) {  
            e.printStackTrace();  
        } finally {  
            if (br!=null) {  
                br.close();  
            }  
            if (wr!=null) {  
                wr.close();  
            }  
        }  
    }  
    public static void main(String[] args) throws IOException {  
        MapReduce();  
    }  
}
```


### hashmap与hashtable

* 1、底层数据结构不同：jdk1.7底层都是数组+链表,但jdk1.8 HashMap加入了红黑树
* 2、Hashtable 是不允许键或值为 null 的，HashMap 的键值则都可以为 null。
* 3、添加key-value的hash值算法不同：HashMap添加元素时，是使用自定义的哈希算法,而HashTable是直接采用key的hashCode()
* 4、实现方式不同：Hashtable 继承的是 Dictionary类，而 HashMap 继承的是 AbstractMap 类。
* 5、初始化容量不同：HashMap 的初始容量为：16，Hashtable 初始容量为：11，两者的负载因子默认都是：0.75。
* 6、扩容机制不同：当已用容量 > 总容量 * 负载因子时，HashMap 扩容规则为当前容量翻倍，Hashtable 扩容规则为当前容量翻倍 +1。
* 7、支持的遍历种类不同：HashMap只支持Iterator遍历,而HashTable支持Iterator和Enumeration两种方式遍历
* 8、迭代器不同：HashMap的迭代器(Iterator)是fail-fast迭代器，而Hashtable的enumerator迭代器不是fail-fast的。所以当有其它线程改变了HashMap的结构（增加或者移除元素），将会抛出ConcurrentModificationException，但迭代器本身的remove()方法移除元素则不会抛出ConcurrentModificationException异常。但这并不是一个一定发生的行为，要看JVM。而Hashtable 则不会。
* 9、部分API不同：HashMap不支持contains(Object value)方法，没有重写toString()方法,而HashTable支持contains(Object value)方法，而且重写了toString()方法
* 10、同步性不同: Hashtable是同步(synchronized)的，适用于多线程环境,而hashmap不是同步的，适用于单线程环境。多个线程可以共享一个Hashtable；而如果没有正确的同步的话，多个线程是不能共享HashMap的。

由于Hashtable是线程安全的也是synchronized，所以在单线程环境下它比HashMap要慢。如果你不需要同步，只需要单一线程，那么使用HashMap性能要好过Hashtable。


### HashMap常用方法以及遍历
|方法|描述|
|:------|:--|
|`put(key, value)`|将指定的值与此映射中的指定键关联并添加到集合中。==如果包含先前的映射，则将旧值替换为新值==|
|`get(key)`|返回指定键所映射的值，如果此映射不包含该键的映射关系，返回null|
|`containsKey(key, value)`|如果此映射包含指定键的映射关系，返回true|
|`containsValue(Object value)`|如果映射中包含有一个或多个键映射到指定值，则返回true|
|`keySet()`|返回此映射中包含的键Set视图|
|`values()`|返回此映射中包含的Collection视图|
|`remove(Object key)`|如果删除成功，返回先前的值，如果失败，返回null|
|`remove(Object key,Object value)`|删除成功返回true,否则返回false|
|`replace(K key,V value)`|仅当指定键映射到某个值时，新值替换，返回旧值|
|`clear()`|清空|
|`clone()`|复制一份 hashMap，属于==浅拷贝==|
|`getOrDefault(key, DefaultValue)`|获取指定 key 对应对 value，如果找不到 key ，则返回设置的默认值|

**HashMap的两种遍历**

```java
public static void Map() {  
    HashMap<Integer, String> map = new HashMap<>();  
    map.put(1, "Jack");  
    map.put(2,"Rose");  
    map.put(3,"Tim");  
    map.put(4, "Tim");  
  
    // 通过key的set视图遍历  
    Set<Integer> keys = map.keySet();  
    for (Integer i : keys) {  
        System.out.println(i + " -> " + map.get(i));  
    }  
  
    // 直接遍历  
    Collection<String> values = map.values();  
    for (String val: values) {  
        System.out.println(val);  
    }  
  
    // 通过迭代器遍历  
    Iterator<Map.Entry<Integer, String>> iterator = map.entrySet().iterator();  
    while (iterator.hasNext()) {  
        Map.Entry<Integer, String> next = iterator.next();  
        System.out.println(next.getKey() + " -> " + next.getValue());  
    }  
  
    // 通过映射的 set 视图遍历（替代迭代器）  
    for (Map.Entry<Integer, String> next : map.entrySet()) {  
        System.out.println(next.getKey() + " -> " + next.getValue());  
    }  
}
```


### HashTable常用方法以及遍历

|方法|描述|
|:------|:--|
|`put(key, value)`|将指定的值与此映射中的指定键关联并添加到集合中。==如果包含先前的映射，则将旧值替换为新值==|
|`get(key)`|返回指定键所映射的值，如果此映射不包含该键的映射关系，返回null|
|`containsKey(key, value)`|如果此映射包含指定键的映射关系，返回true|
|`containsValue(Object value)`|如果映射中包含有一个或多个键映射到指定值，则返回true|
|`keySet()`|返回此映射中包含的键Set视图|
|`remove(Object key)`|如果删除成功，返回先前的值，如果失败，返回null|
|`replace(K key,V value)`|仅当指定键映射到某个值时，新值替换，返回旧值|
|`clear()`|清空|
|`clone()`|复制一份 hashTable，属于==浅拷贝==|
|`getOrDefault(key, DefaultValue)`|获取指定 key 对应对 value，如果找不到 key ，则返回设置的默认值|
|`elements( )`|返回表中==值的枚举==|
|`keys()`|返回==键的枚举==|
|`toString()`|返回此 Hashtable 对象的字符串表示形式，其形式为 ASCII 字符 ", " （逗号加空格）分隔开的、括在括号中的一组条目|

**HashTable四种遍历方式**

```java
public static void Table() {  
    Hashtable<String, Integer> table = new Hashtable<>();  
    table.put("a", 1);  
    table.put("b", 2);  
    table.put("c", 3);  
    table.put("d", 4);  
    table.put("e", 5);  
    table.put("f", 6);  
  
    // 获取键的枚举遍历 HashTable    Enumeration<String> keys = table.keys();  
    while (keys.hasMoreElements()) {  
        String str = keys.nextElement();  
        System.out.println(str + " -> " + table.get(str));  
    }  
  
    // 使用lambda表达式遍历 HashTable    
    table.forEach((K, V) -> System.out.println(K + " -> " + V));  
  
    // 使用 key 集合的 set 视图  
    Set<String> keys_1 = table.keySet();  
    for (String k : keys_1) {  
        System.out.println(k + " -> " + table.get(k));  
    }  
  
    // 使用值枚举直接遍历值  
    Enumeration<Integer> values = table.elements();  
    while (values.hasMoreElements()) {  
        System.out.println(values.nextElement());  
    }  
}
```




