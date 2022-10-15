## Python数据结构与算法
### 二分查找

O(log(n))

要求***有序列表***，必须要*先排序*

如果是无序列表，而且查找值比较少，不建议二分查找

![二分查找](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg-blog.csdnimg.cn%2F20210222214705314.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4OTI5NDMz%2Csize_16%2Ccolor_FFFFFF%2Ct_70&refer=http%3A%2F%2Fimg-blog.csdnimg.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644703955&t=7171fb209b7ca7506d0c68bed0d35ce6)
```python
def cha(arr: list[int], key: int) -> int:
    """
    二分查找，
    先将被查找的列表排序（升降都可）
    找中间的数值与查找的比较，如果相等即返回
    如果大于就在左边数列继续上面的操作
    如果小于就在右边数列继续上面的操作
    :param arr:被查找的数列
    :param key:要查找的数
    :return:要查找的数的索引
    """
    i = 0
    high = len(arr) - 1
    while i <= high:
        # 取中间的一个值,防止溢出
        mid = i + (high - i) // 2
        if arr[mid] == key:
            return mid
        # 如果mid的数大于要查找的数就在 i ~ mid - 1之间找mid重复操作
        elif arr[mid] > key:
            n = mid - 1
        # 如果mid的数小于要查找的数就在 mid + 1 ~ high 之间找mid重复操作
        elif arr[mid] < arr:
            i = mid + 1
```

递归二分查找

```python
def cha(nums: list[int], target: int, start: int, end: int):
    start = 0
    end = len(arr) - 1
    mid = (start + end) // 2

    if arr[mid] == target:
        return mid
    elif arr[mid] > target:
        return cha(arr, target, start, mid - 1)
    elif arr[mid] < target:
        return cha(arr, target, mid + 1, end)
```

### 排序

![排序算法](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimage.mamicode.com%2Finfo%2F201810%2F20181017232651322830.jpg&refer=http%3A%2F%2Fimage.mamicode.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644704615&t=ac8ea5b73cfd14cb1678dce3974d5193)

### 1、冒泡排序

复杂度较高

如果在有一趟中没有发生任何交换可以认为已经排好了

```python
def maopao(nums: list[int]):
    """"
    冒泡排序
    比较两个相邻的数，如果不符合需要的排序（升序或降序）则交换两数
    冒泡排序一共会运行len（nums）-1趟（最后一趟不用比）
    每循环一趟会出来一个符合排序规则的数（在有序区）
    每次循环只在无序区排序
    有序去范围:i + 1
    无序区范围:len(nums) - i - 1     （第 i 趟）
    :param nums:要排序的列表
    :return:拍好的列表
    """
    # 第 i 趟
    for i in range(len(nums)-1):
        exchange = False  # 优化，当没有交换一次的时候直接返回
        # 在无序区范围排序
        for j in range(len(nums)-i-1):
            # 比较相邻的两个数，不符合排序规则的交换
            if nums[j] > nums[j+1]:  # 大于号改为小于号即为降序
                nums[j], nums[j+1] = nums[j+1], nums[j]
        if not exchange:
            return nums
            # print("第 %d 趟：" % i, nums)
    return nums
```

### 2、选择排序    
![选择排序](https://img2.baidu.com/it/u=3364014278,802276825&fm=253&fmt=auto&app=138&f=GIF?w=480&h=452)
```python
def xuanze(nums: list[int]):
    """
    假定第一个数为最小值，如果在后面发现更小的则交换，
    然后在无序区重复以上操作
    :param nums: 要排序的数列
    :return: 排序好的数列
    """
    # 需要 len(nums) - 1 趟
    for i in range(len(nums) - 1):
        min_loc = i       # 假定无序区第一个位置的数是最小值
        # 无序区的范围
        for j in range(i+1, len(nums)):
            # 当发现j位置的数比假定的小就交换
            if nums[j] < nums[min_loc]:
                min_loc = j     # 将j位置设为最小
        nums[i], nums[min_loc], = nums[min_loc], nums[i]
    return nums
```

### 3、插入排序    
![插入排序](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg.136.la%2F20210718%2Fe728ce7200624a7d89fe33ede1969550.jpg&refer=http%3A%2F%2Fimg.136.la&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644705160&t=ab3ca55c5b7b132bff912b0c2af6f844)
```python
def charu(nums: list[int]):
    # i 表示要排序的下标
    for i in range(1, len(nums)):
        tmp = nums[i]
        j = i - 1   # 排好的下标
        # 要排的比排好的最右边的数大而且没有比到最左边
        while nums[j] > tmp and j >= 0:
            nums[j+1] = nums[j]     # 向左移一位
            j -= 1
        nums[j+1] = tmp
    return nums
```

### 4、快速排序

![快速排序](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fbkimg.cdn.bcebos.com%2Fpic%2F574e9258d109b3dee4ddfa6acfbf6c81800a4c55&refer=http%3A%2F%2Fbkimg.cdn.bcebos.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644705453&t=affa05f48f52e6fc3bb5ba53a433705d)
```python
def kuaisu(nums: list[int], left: int, right: int) -> list[int]:
    # 保存要排序的最左边的数
    tmp = nums[left]
    while left < right:
        while nums[right] >= tmp and left < right:
            right -= 1
        nums[left] = nums[right]
        while nums[right] <= tmp and left < right:
            left += 1
        nums[left] = nums[right]

    nums[left] = tmp

    return nums
```

***递归快速排序***

```python
def kuaipai(nums: list[int], left: int, right: int) -> list[int]:
    # temp = nums[left]
    if left < right:
        # mid = kuaisu(nums, left, right)
        kuaipai(nums, left, right-1)
        kuaipai(nums, left+1, right)

    return nums
```

### 5、归并排序

1- 确定分界点

2- 递归排序 left right

3- 归并 —— 合二为一   

![归并排序](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimage.mamicode.com%2Finfo%2F201905%2F20190513091756164239.png&refer=http%3A%2F%2Fimage.mamicode.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644705541&t=1ea092d46a612ce4d8da19a1de3a7ea6)
```python
def guibing(nums: list[int], left: int, right: int) -> list[int]:
    mid = int((left+right) // 2)
    j = mid + 1
    tmp = []    # 用于存储排序好的
    k = 0       # 记录次数
    while left <= mid and j <= right:

        if nums[left] <= nums[j]:
            k += 1
            left += 1
            tmp[k] = nums[left]

    return nums
```

### 6、堆排序

#### 基本知识

##### 1、树
树是一种可递归定义的数据结构

树是由n个节点组成的集合（递归定义）：

如果n = 0，那这是一颗空树

如果n > 0，那么存在1个节点作为根节点，其他节点分为m个集合，每个集合本身又是棵树

* 1、根节点、子节点、叶子节点：树的最顶端为根节点，根节点往下分为子节点，直到后面没有子节点的节点为叶子节点
* 2、树的深度（高度）：根节点到最下一层的层数
* 3、树的度：这个节点有几个子节点，度就是几，节点的最大度就是树的度
* 4、孩子节点、父节点：节点是子节点的父节点，子节点是节点的孩子节点
* 5、子树：一个树的一个分支都是子树

#### 2、二叉树

* 度不超过 2 的树就是二叉树（每个节点最多只有两个孩子节点，分别为左孩子节点和右孩子结点）
* 满二叉树：如果每层的节点数都到达了最大值
* 完全二叉树：叶子节点只能出现在最下层和次下层，并且最下层的节点都集中在最左边的若干位置

#### 二叉树的存储方式（表示方式）

* 链式存储方式：用链表表示
* 顺序存储方式：用列表表示，从根节点开始为 0 ，按层往下，从左往右依次增加

  >父节点编号与左孩子节点编号 i -> 2i + 1，与左孩子节点 i -> 2i + 2
  
  
### 堆排序
* 1、堆：一种特殊的完全二叉树

  大根堆：一颗完全二叉树，满足任一节点都比其他孩子节点大
  
  小根堆：一颗完全二叉树，满足任一节点都比其他孩子节点小
  
  
![堆](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg-blog.csdnimg.cn%2F2021042907094577.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoZW5sb25nX2N4eQ%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70&refer=http%3A%2F%2Fimg-blog.csdnimg.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644703328&t=f6aa7ed18ad21aac5d1848748c1bec29)
* 2、堆的向下调整性质

  假设根节点的左右子树都是堆，但跟节点不满足堆的性质，那么可以通过一次向下的调整来将其变成一个堆
  
* 3、堆的向下取整
* 
  拿去根节点，将孩子节点中较大的补上去，依次重复操作，直到拿出的根节点的数有放入的空位
  
  ![堆的向下取整](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg-blog.csdnimg.cn%2F2020112819400047.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NQZW5fd2Vi%2Csize_16%2Ccolor_FFFFFF%2Ct_70&refer=http%3A%2F%2Fimg-blog.csdnimg.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644703328&t=9b935ec60a1911686ed123ea4723eeda)

#### 堆排序
* 1、建立堆
* 2、得到堆顶元素为最大元素
* 3、去掉堆顶，将堆最后一个元素放到堆顶，此时可通过一次调整重新使堆有序
* 4、堆顶元素为第二大元素
* 5、重复步骤 3 ，直到堆变空\

***向下取整函数实现***

```python
def sift(li: list[int], low: int, high: int):
	"""
	:param li: 要排序的数组
	:param low: 堆的根节点位置
	:param high: 堆的最后一个元素的位置
	"""
	i = low         # i 初始化指向根节点
	j = 2 * i + 1   # j 初始化为根节点的左孩子结点 
	tmp = li[i]     # 将堆顶保存起来

	# 只要 j 的位置不为空(没超过最后一个节点位置)
	while j <= high:
		# 如果右孩子存在且有孩子大于左孩子
		if li[j + 1] > li[j] and j + 1 <= high:
			j = j + 1   # 指向右孩子
		# 比较 tmp 和 li[j]（较大的孩子节点），较大的放到堆顶
		if li[j] > tmp:
			li[i] = li[j]
			i = j           # 更新 i 的位置（往下看一层）
			j = 2 * i + 1   # 更新 j 的位置（新 i 的左孩子节点）
		else:       # tmp 更大，将 tmp 放到 i 的位置上
			li[i] = tmp
			break
	else:       # 当 j 大于 high 时跳出循环，此时叶子节点为空，将 tmp 放到 i 位置
		li[i] = tmp
``` 

***堆排序函数的实现***

```python
def heap_sort(li: list[int]) -> list[int]:
	n = len(li)
	# 从最后一个开始向前建堆
	for i in range((n - 2) // 2, -1, -1):
		# i 表示建堆的时候调整的部分的根的下标
		sift(li, i, n - 1)
	for i in range(n - 1, -1, -1):
		li[0], li[i] = li[i], li[0]
		
```


### 动态规划
* 分类
   * 基础问题：爬楼梯、斐波那契数列
   * 背包问题
   * 打家劫舍
   * 股票问题
   * 子序列问题

* 解题步骤
   * 1、dp数组以及下标 i（ j ） 的含义
   * 2、递推公式
   * 3、dp数组如何初始化
   * 4、遍历顺序
   * 5、打印dp数组

>打印杨辉三角

| 行\列 |   0   |   1   |   2   |   3   |   4   |
| :---: | :---: | :---: | :---: | :---: | :---: |
|   0   |   1   |       |       |       |       |
|   1   |   1   |   1   |       |       |       |
|   2   |   1   |   2   |   1   |       |       |
|   3   |   1   |   3   |   3   |   1   |       |
|   4   |   1   |   4   |   6   |   4   |   1   |

* 1、二维dp数组，i，j代表第i行第j列的数字
* 2、`dp[i][j] = dp[i-1][j-1] + dp[i-1][j]`
* 3、初始化0，1行和0列和[i][i]为1，故直接初始化全部为1
* 4、由图和递推公式，很显然先放列再放行，故先循环行，后循环列，且row从2 开始 n 结束，col从1开始 row-1 结束
* 5、打印dp数组

```python
def generate(numRows: int) -> List[List[int]]:
	# 将每一行每一个值初始化为1
	dp = [[1] * i for i in range(1, numRows + 1)]

	# 第i行的中间（2 ~ i-1）位置的值
	for i in range(2, numRows):

		for j in range(1, i):
			dp[i][j] = dp[i-1][j-1] + dp[i-1][j]

	return dp
```

