## 概念
ArrayList 类是一个可以动态修改的数组，与普通数组的区别就是它是==没有固定大小==的限制，我们可以添加或删除元素。

ArrayList 继承了 AbstractList ，并实现了 List 接口

![](https://www.runoob.com/wp-content/uploads/2020/06/ArrayList-1-768x406-1.png)

## 常用方法

|方法|描述|
|:--|:--|
|`add(?index, value)`|默认在末尾添加元素，索引不能为负且必须存在。若不指定index，返回bool|
|`clear()`|清空|
|`clone()`|浅拷贝|
|`contains(value)`|是否存在此元素|
|`get(index)`|通过索引获取，不可以使用负索引，索引必须存在|
|`indexOf(value)`|找到第一个值为value的索引，不存在返回 -1|
|`remove(index)`|删除指定索引值|
|`size()`|返回元素个数|
|`isEmpty()`|返回是否为空|
|`subList(startIndex, endIndex)`|切片|
|`set(index, newValue)`|替换index处元素，返回被替换元素|
|`sort()`|排序|
|`forEach()`|对每一个元素进行相同操作|
|`lastIndexOf(value)`|找到最后值为value的索引，不存在返回 -1|


## 遍历

```java
public static void arrayList() {  
    ArrayList<Integer> arr = new ArrayList<>();  
    for (int i = 0; i < 10; i++) {  
        arr.add(i);  
    }  
  
    // 普通遍历  
    for (int i = 0; i < arr.size(); i++) {  
        System.out.printf("%d\t", i);  
    }  
    System.out.println();  
  
    // 增强遍历  
    for (Integer i : arr) {  
        System.out.printf("%d\t", i);  
    }  
    System.out.println();  
  
    // forEach 遍历  
    arr.forEach((value) -> System.out.printf("%d\t", value));  
    System.out.println();  
}
```