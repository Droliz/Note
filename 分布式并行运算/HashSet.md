### HashSet

HashSet 基于 HashMap 来实现的，是一个不允许有重复元素的集合，允许有 null 值，是无序的，即不会记录插入的顺序。 不是线程安全的， 如果多个线程尝试同时修改 HashSet，则最终结果是不确定的。 您必须在多线程访问时显式同步对 HashSet 的并发访问。HashSet 实现了 Set 接口

|方法|描述|
|:--|:--|
|`add(value)`|添加元素|
|`clear()`|清空 hashset|
|`clone()`|浅拷贝|
|`contains(value)`|检查此HashSet中给定对象(ob)是否存在|
|`isEmpty()`|判断是否为空|
|`iterator()`|返回hashset的迭代器|
|`remove(Value)`|删除值，返回bool|
|`size()`|返回大小|


**hashset遍历**

```java
public static void Set() {  
    HashSet<String> sites = new HashSet<>();  
    sites.add("Google");  
    sites.add("python");  
    sites.add("html");  
    sites.add("css");  
    sites.add("java");  
  
    // 直接遍历  
    for (String i : sites) {  
        System.out.println(i);  
    }  
  
    // 使用迭代器遍历  
    for (Iterator<String> iterator = sites.iterator(); 
	    iterator.hasNext(); ) {  
        System.out.println(iterator.next());  
    }  
  
    // 转换为数组遍历  
    String[] arr = sites.toArray(new String[]{});  
    for (String str:arr) {  
        System.out.println(str);  
    }  
}
```
