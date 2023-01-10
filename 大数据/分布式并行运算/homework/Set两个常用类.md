
## HashSet

> HashSet：使用哈希表的存储方式，HashMap实现；散列存放,无法保证原来的存储顺序

HashSet是允许null值的，

所以 HashSet 适用于需要很快获取数据，而且不涉及排序的场景（仅注重快速）

```java
public static void hashSet() {  
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


## TreeSet

>TreeSet 底层数据结构是红黑树，元素是有序的，排序的方式有自然排序和定制排序

TreeSet是不允许null值的，自定义类需要实现comparable接口，重写comparaTo() 方法

所以 TreeSet 适用于需要对数据进行**自定义**排序的场景（尽可能的快，而且自定义排序）

例如：升序、降序或按数据某一个字段进行升，然后再按另一个字段进行降序等


对于基本数据类型

```java
public static void treeSet() {  
    TreeSet<String> sites = new TreeSet<>();  
    sites.add("Google");  
    sites.add("python");  
    sites.add("html");  
    sites.add("css");  
    sites.add("java");  
    for (String i : sites) {  
        System.out.printf("%s\t", i);  
    }  
}
```

自定义的类实例对象

```java
class People implements Comparable<People>{  
    private String name;  
    private int age;  
  
    public People(String name, int age) {  
        this.name = name;  
        this.age = age;  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public int getAge() {  
        return age;  
    }  
  
    public void setAge(int age) {  
        this.age = age;  
    }  
  
    @Override  
    public boolean equals(Object o) {  
        if (this == o) return true;  
        if (o == null || getClass() != o.getClass()) return false;  
        People people = (People) o;  
        return age == people.age &&  
                Objects.equals(name, people.name);  
    }  
  
    @Override  
    public String toString() {  
        return "People{" +  
                "name='" + name + '\'' +  
                ", age=" + age +  
                '}';  
    }  
  
    @Override  
    public int hashCode() {  
        return Objects.hash(name, age);  
    }  
  
    public People() {  
        super();  
    }  
  
    @Override  
    public int compareTo(People o) {  
        //this 与 o比较  
        //返回数据：负数 （this小）/ 零（一样大）/正数 this 大  
        if(this.age>o.age){  
            return 1;  
        }else if(this.age<o.age){  
            return  -1;  
        }else{  
            return 0;  
        }  
  
    }
}
```



```java
public static void treeSet() {  
    TreeSet<Person> p = new TreeSet<>();  
  
    Person p1 = new Person("张三",18);  
    Person p2 = new Person("李四",20);  
    //People p3 = new People("悠悠",20);  
    //TreeSet不会存储一样的数据，p3不会被存储进去  
    p.add(p1);  
    p.add(p2);  
  
    for(Person per :p){  
        System.out.println(per);  
    }  
}
```