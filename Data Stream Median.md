# Data Stream Median

Data Stream Median  ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/data-stream-median/) )

```
Description
Numbers keep coming, return the median of numbers at every time a new number added.

Clarification
What's the definition of Median?
- Median is the number that in the middle of a sorted array. 
If there are n numbers in a sorted array A, the median is A[(n - 1) / 2]. 
For example, if A=[1,2,3], median is 2. If A=[1,19], median is 1.

Example
For numbers coming list: [1, 2, 3, 4, 5], return [1, 1, 2, 2, 3].
For numbers coming list: [4, 5, 1, 3, 2, 6, 0], return [4, 4, 4, 3, 3, 3, 3].
For numbers coming list: [2, 20, 100], return [2, 2, 20].

Challenge 
Total run time in O(nlogn).
```



### 解题思路

#### 一、优先队列/堆

- 使用两个堆 `Heap` 和一个变量 `median`，`median`存储当前的中位数，最大堆 `maxHeap` 存储小于 `median` 的数，最小堆 `minHeap` 存储大于 `median` 的数。
- 新插入数值比 `median` 小，放入 `maxHeap`；比 `median` 大，放入 `minHeap` 。
- 比较 `maxHeap` 和 `minHeap` 的大小，本解法中其实是比较 `maxHeap.size() + 1` 和 `minHeap` 的大小。
  - 如果 `maxHeap.size() > minHeap.size()` ，那么将 `median` 放入 `minHeap` ，取出 `maxHeap` 最大值作为当前 `median` 。
  - 如果 `maxHeap.size() + 1 < minHeap.size()` ，那么将 `median` 放入 `maxHeap` ，取出 `minHeap` 最小值作为当前 `median` 。

其中，Java 中的 `PriorityQueue` 默认是最小堆，堆顶元素是最小值，所以处理最大堆时取了负数。

Java 实现

```java
public class Solution {
    /**
     * @param nums: A list of integers.
     * @return: the median of numbers
     */
    public int[] medianII(int[] nums) {
        int[] results = new int[nums.length];
        if (nums == null || nums.length == 0) {
            return results;
        }
        
        int median = nums[0];
        results[0] = median;
        PriorityQueue<Integer> maxHeap = new PriorityQueue<Integer>(); 
        PriorityQueue<Integer> minHeap = new PriorityQueue<Integer>();
        
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] < median) {
                maxHeap.add(-nums[i]);
            } else {
                minHeap.add(nums[i]);
            }
            
            if (maxHeap.size() > minHeap.size()) {
                minHeap.add(median);
                median = -maxHeap.peek();
                maxHeap.remove();
            } else if (maxHeap.size() + 1 < minHeap.size()) {
                maxHeap.add(-median);
                median = minHeap.peek();
                minHeap.remove();
            }
            
            results[i] = median;
        }
        
        return results;
    }
}
```



#### 二、优先队列/堆 II

和方法一的实现思路类似，具体实现细节有所不同。不再单独使用变量存储 median ，维持 maxHeap 和 minHeap 的大小，maxHeap 可以比 minHeap 多一个元素， maxHeap 堆顶即为中位数 median 。

本方法的实现更加直观。

Java 实现

```java
public class Solution {
    /**
     * @param nums: A list of integers.
     * @return: the median of numbers
     */
    public int[] medianII(int[] nums) {
        int[] results = new int[nums.length];
        if (nums == null || nums.length == 0) {
            return results;
        }
        
        //left half and the median are stored in the maxHeap. median == maxHeap.peek()
        //right half of the median are stored in the minHeap.
        PriorityQueue<Integer> minHeap = new PriorityQueue<Integer>( );//default min heap
        PriorityQueue<Integer> maxHeap = new PriorityQueue<Integer>(11 /*default capacity*/, new Comparator<Integer>() {
            @Override
            public int compare(Integer x, Integer y) {
                return y - x;
            }
        });
        
        results[0] = nums[0];
        maxHeap.add(nums[0]);
        
        for (int i = 1; i < nums.length; i++) {
            int tmp = maxHeap.peek();
            if (nums[i] > tmp) {
                minHeap.add(nums[i]);
            } else {
                maxHeap.add(nums[i]);
            }
            
            //maxHeap.size() == minHeap.size() || maxHeap.size() == minHeap.size() + 1;
            if (maxHeap.size() > minHeap.size() + 1) {
                minHeap.add(maxHeap.poll());
            } else if (maxHeap.size() < minHeap.size()) {
                maxHeap.add(minHeap.poll());
            }
            results[i] = maxHeap.peek();
        }
        
        return results;
    }
}
```



### 参考

1. [Data Stream Median | 九章算法](http://www.jiuzhang.com/solutions/median-in-data-stream/)
2. [lintcode 1: Data Stream Median | 西施豆腐渣](http://blog.csdn.net/xudli/article/details/46389077)
3. [Data Stream Median – Lintcode Java | WELKIN LAN](http://blog.welkinlan.com/2015/06/26/data-stream-median-lintcode-java/)

