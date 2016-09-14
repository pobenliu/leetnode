# Heapify

Heapify  ( [leetcode]() [lintcode]() )

```
Description
Given an integer array, heapify it into a min-heap array.
For a heap array A, A[0] is the root of heap, and for each A[i], A[i * 2 + 1] is the left child of A[i] and A[i * 2 + 2] is the right child of A[i].

Clarification
- What is heap?
- Heap is a data structure, which usually have three methods: push, pop and top. where "push" add a new element the heap, "pop" delete the minimum/maximum element in the heap, "top" return the minimum/maximum element.

- What is heapify?
- Convert an unordered integer array into a heap array. If it is min-heap, for each element A[i], we will get A[i * 2 + 1] >= A[i] and A[i * 2 + 2] >= A[i].

- What if there is a lot of solutions?
- Return any of them.

Example
Given [3,2,1,4,5], return [1,2,3,4,5] or any legal heap array.

Challenge 
O(n) time complexity
```



### 解题方法

构造堆的基本操作有两种：`siftdown` 和 `siftup` 。两种操作在本质上是相同的，处理不符合堆定义的结点直至满足规则。以构造最小堆为例：

- `siftup` ：当前结点比父结点小，两者交换，迭代操作直至当前结点大于其父结点。
- `siftdown` ：当前结点比父结点大，同较小的儿子结点交换，迭代操作直至当前结点大于两个儿子结点。

对一个结点进行 `siftup` 或 `siftdown` 时涉及的实际操作数，与该结点需要移动的距离正相关。对 `siftup` 是结点到树底层的距离，所以树顶端结点的移动代价较高；对 `siftdown` 是结点到树的根结点的距离，所以叶子节点的移动代价较高。不难发现，自顶向下 `siftdown` 构造堆时，只有根结点等少数几个结点需要移动约为 `logn` 的长距离；而自底向上 `siftdown` 构造堆时，大量的叶子结点需要移动约为 `logn` 的距离，所以两种方式构造堆所需要的操作数是不同的。

假设树的高度为 `h = logn` ，那么在最坏情况下，自顶向下 `siftdown` 构造堆所需要的操作数为：

> `sum1 = (0 * n/2) + (1 * n/4) + (2 * n/8) + ... + (h * 1)` 

使用泰勒级数对其进行近似可以得到时间复杂度最坏为 `O(n)` 。具体证明过程请参考[链接](http://www.cs.umd.edu/~meesh/351/mount/lectures/lect14-heapsort-analysis-part.pdf)。

同样假设在最坏情况下，自底向上 `siftdown` 构造堆所需要的操作数为：

> `sum2 = (h * n/2) + ((h-1) * n/4) + ((h-2)*n/8) + ... + (0 * 1)` 

 `siftdown` 所需要的操作更多，其中第一项 `h * n/2 = 1/2 * nlogn` ，因此其复杂度最差为 `O(nlogn)` 。

以上内容其实会引出一个问题：

> 如果构造堆的时间复杂度为 `O(n)` ，那么堆排序的时间复杂度为什么是 `O(nlogn)` 呢？

具体分析可参考本文提供的参考链接。

#### 一、自底向上 `siftup`

根据最小堆的定义，对数组中的每个元素，依次进行 `siftup` 操作。

- `siftup` ：当前结点比父结点小，两者交换，迭代操作直至当前结点大于其父结点。



Java 实现

```java
public class Solution {
    /**
     * @param A: Given an integer array
     * @return: void
     */
    public void heapify(int[] A) {
        if (A == null || A.length == 0) {
            return;
        }
        
        for (int i = 0; i < A.length; i++) {
            siftup(A, i);
        }
    }
    
    private void siftup (int[] A, int k) {
        while (k != 0) {
            int father = (k - 1) / 2;
            if (A[k] > A[father]) {
                break;
            }
            int temp = A[k];
            A[k] = A[father];
            A[father] = temp;
            
            k = father;
        }
    }
}
```



#### 二、自顶向下 `siftdown` 

对数组中的每个元素，依次进行 `siftdown` 操作，可参考这个[演示 demo](https://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/DemoHeapify.pdf)。

- `siftdown` ：当前结点比父结点大，同较小的儿子结点交换，迭代操作直至当前结点大于两个儿子结点。

注：具体实现时需要注意索引的边界问题。

Java 实现

```java
public class Solution {
    /**
     * @param A: Given an integer array
     * @return: void
     */
    public void heapify(int[] A) {
        if (A == null || A.length == 0) {
            return;
        }
        // the heap array start at index 0, not index 1 (i = A.length / 2)
        for (int i = A.length / 2 - 1; i >= 0; i--) {
            siftdown(A, i);
        }
    }
    
    private void siftdown (int[] A, int k) {
        while (k < A.length) {
            int smallest = k;
            if (k * 2 + 1 < A.length && A[k * 2 + 1] < A[smallest]) {
                smallest = k * 2 + 1;
            }
            if (k * 2 + 2 < A.length && A[k * 2 + 2] < A[smallest]) {
                smallest = k * 2 + 2;
            }
            if (smallest == k) {
                break;
            }
            
            int tmp = A[smallest];
            A[smallest] = A[k];
            A[k] = tmp;
            
            k = smallest;
        }
    }
    
}
```



### 参考

1. [ Heapify | 九章算法](http://www.jiuzhang.com/solutions/heapify/)

2. [How can building a heap be O(n) time complexity? | StackOverFlow](http://stackoverflow.com/questions/9755721/how-can-building-a-heap-be-on-time-complexity)

3. [Heapify | LintCode & LeetCode 题解分析](https://aaronice.gitbooks.io/lintcode/content/data_structure/heapify.html)

   ​