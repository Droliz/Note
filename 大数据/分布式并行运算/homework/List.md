## 一、List 概述
List 是对有序集合的分装，可对其中每个元素的插入位置进行精确地控制，并通过索引来访问、遍历元素。List 集合中，常用的是 ArrayList 和 LinkedList 这两个类。其中，ArrayList 底层通过数组实现，随着元素的增加而动态扩容，LinkedList底层通过双向链表来实现，随着元素的增加不断向链表的后端增加节点。

## 二、ArrayList
ArrayList 内部使用数组实现，对操作数组时的数据位移和插入数据时的数组扩容进行了封装。适合删除数据少，查询数据多的场景。

### 2.1 add 添加元素
插入数据，插入前校验是否已经超出了数组容量，超出时进行数组扩容，扩容数组容量增量为原数组容量的 1/2，不能手动设置扩容的增量。

```java
public boolean add(E e) {
    // 如果需要则进行数组扩容
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    // 插入数据
    elementData[size++] = e;
    return true;
}

// 如果minCapacity大于数组长度，则进行扩容
private void ensureCapacityInternal(int minCapacity) {
    ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
}

// 如果未指定默认数组长度，且未初始化数组，最小数组长度为DEFAULT_CAPACITY
private static int calculateCapacity(Object[] elementData, int minCapacity) {
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        // 实例化时未指定默认数组长度，且未初始化数组
        return Math.max(DEFAULT_CAPACITY, minCapacity);
    }
    return minCapacity;
}

// 增加修改次数，如果minCapacity超过数组长度进行数组扩容
private void ensureExplicitCapacity(int minCapacity) {
    // 修改次数加1
    modCount++;
    // minCapacity大于数组长度
    if (minCapacity - elementData.length > 0)
        // 进行数组扩容
        grow(minCapacity);
}

// 增加容量以确保它至少可以容纳由minCapacity指定的元素数量
private void grow(int minCapacity) {
    // overflow-conscious code
    // 取得原数组长度
    int oldCapacity = elementData.length;
    // 数组长度增加1/2
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    // 最小值设置为minCapacity
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    // 大于指定的最大值
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        // 检查minCapacity是否小于0，否则将数组长度设置为最大值
        newCapacity = hugeCapacity(minCapacity);
    // 数组扩容
    elementData = Arrays.copyOf(elementData, newCapacity);
}
```


### 2.2 get 查询元素
```java
public E get(int index) {
    rangeCheck(index);
    return elementData(index);
}
// 如果index超出数组范围则抛出异常
private void rangeCheck(int index) {
    if (index >= size)
        throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
}
// 取得指定下标的元素
E elementData(int index) {
    return (E) elementData[index];
}
```

### 2.3 remove 删除元素

删除元素，并将被删除元素后面的元素前移，删除元素时数组并不会进行数组收缩。

```java
public boolean remove(Object o) {
    if (o == null) {
        // 待删除元素为null，从前往后遍历找到第一个为null的元素
        for (int index = 0; index < size; index++)
            if (elementData[index] == null) {
                fastRemove(index);
                return true;
            }
    } else {
        // 通过equals比较，从前往后遍历找到第一个与o相同的元素
        for (int index = 0; index < size; index++)
            if (o.equals(elementData[index])) {
                fastRemove(index);
                return true;
            }
    }
    return false;
}
// index下标之后的元素前移一位
private void fastRemove(int index) {
    // 增加修改次数
    modCount++;
    // 计算需要移动的长度
    int numMoved = size - index - 1;
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index,
                         numMoved);
    // 失效的下标设置为null
    elementData[--size] = null; // clear to let GC do its work
}
```

## 三、Vector
Vector 整体的实现逻辑和 ArrayList 相同，但是在方法上都使用了 synchronized ，Vector 所以是线程安全的 List 。

Collections 类的 synchronizedList 方法也是通过 synchronized 实现同步。方法采用了代理模式，在调用 List 的方法前进行加锁。

```java
// 添加元素
public synchronized boolean add(E e) {
    // 修改次数加1
    modCount++;
    // 如果需要则进行数组扩容
    ensureCapacityHelper(elementCount + 1);
    // 插入数据
    elementData[elementCount++] = e;
    return true;
}
// 查询元素
public synchronized E get(int index) {
    // 如果index超出数组范围则抛出异常
    if (index >= elementCount)
        throw new ArrayIndexOutOfBoundsException(index);
    // 取得指定下标数据
    return elementData(index);
}
// 删除元素
public boolean remove(Object o) {
    return removeElement(o);
}
public synchronized boolean removeElement(Object obj) {
    // 添加修改次数
    modCount++;
    // 取得元素对应的下标
    int i = indexOf(obj);
    if (i >= 0) {
        // index之后的元素前移
        removeElementAt(i);
        return true;
    }
    return false;
}
```


## 四、LinkedList
LinkedList 内部通过双向链表来实现，对链表的增删改查进行了封装。相比于 ArrayList 优势是删除和插入数据时无须进行数组位移，效率更高；缺点是更加损耗内存，数组遍历速度更慢。

### 4.1 add 添加元素
```java
public boolean add(E e) {
    // 节点插入到尾部
    linkLast(e);
    return true;
}

void linkLast(E e) {
    // 取得最后一个节点
    final Node<E> l = last;
    // last后面添加节点
    final Node<E> newNode = new Node<>(l, e, null);
    // 新增的节点设置为last
    last = newNode;
    if (l == null)
        // 前置节点为null，设置为第一个节点
        first = newNode;
    else
        // 设置前置节点的next引用
        l.next = newNode;
    // 长度+1
    size++;
    // 修改次数+1
    modCount++;
}
```


### 4.2 get 查询元素
```java
public E get(int index) {
    // 检查index是否超出数组长度
    checkElementIndex(index);
    // 遍历到第index个节点，取得item
    return node(index).item;
}

Node<E> node(int index) {
    // assert isElementIndex(index);
    // index小于长度的二分之一，从前往后遍历
    if (index < (size >> 1)) {
        Node<E> x = first;
        // 循环遍历找到第index个节点
        for (int i = 0; i < index; i++)
            x = x.next;
        return x;
    } else {
        // 从后往前遍历
        Node<E> x = last;
        // 循环遍历找到第index个节点
        for (int i = size - 1; i > index; i--)
            x = x.prev;
        return x;
    }
}
```


### 4.3 remove 删除元素
```java
public E remove(int index) {
    // 检查index是否超出数组长度
    checkElementIndex(index);
    // 找到第index个元素通过unlink删除这个元素的引用
    return unlink(node(index));
}
E unlink(Node<E> x) {
    // assert x != null;
    final E element = x.item;
    // 后置节点
    final Node<E> next = x.next;
    // 前置节点
    final Node<E> prev = x.prev;
    if (prev == null) {
        // 前置节点为null，将后置节点设置为第一个节点
        first = next;
    } else {
        // 设置前置节点的后置引用
        prev.next = next;
        x.prev = null;
    }
    if (next == null) {
        // 后置节点为null，将前置接地那设置为最后一个节点
        last = prev;
    } else {
        // 设置后置接地那的前置引用
        next.prev = prev;
        x.next = null;
    }
    // 删除item引用
    x.item = null;
    // 数组长度-1
    size--;
    // 修改次数+1
    modCount++;
    return element;
}
```

