#  Median of two Sorted Arrays

 Median of two Sorted Arrays  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/median-of-two-sorted-arrays/#) )

```
Description
There are two sorted arrays A and B of size m and n respectively. 
Find the median of the two sorted arrays.

Example
Given A=[1,2,3,4,5,6] and B=[2,3,4,5], the median is 3.5.
Given A=[1,2,3] and B=[4,5], the median is 3.

Challenge 
The overall run time complexity should be O(log (m+n)).
```

### 解题思路

中位数的概念（[维基百科](https://zh.wikipedia.org/zh-cn/%E4%B8%AD%E4%BD%8D%E6%95%B8)）：

>对于一组有限个数的数据来说，它们的中位数（ Median ）是这样的一种数：这群数据里的一半的数据比它大，而另外一半数据比它小。
>
>计算有限个数的数据的中位数的方法是：把所有的同类数据按照大小的顺序排列。如果数据的个数是**奇数**，则中间那个数据就是这群数据的中位数；如果数据的个数是**偶数**，则中间那2个数据的算术平均值就是这群数据的中位数。

#### 一、归并排序

比较直观的方法是将两个排序数组合并为一个排序数组，然后再求中位数。

##### 算法复杂度

- 时间复杂度： `O(m + n)` 。
- 空间复杂度： `O(m + n)` 。

Java 实现

```java
class Solution {
    /**
     * @param A: An integer array.
     * @param B: An integer array.
     * @return: a double whose format is *.5 or *.0
     */
    public double findMedianSortedArrays(int[] A, int[] B) {
        if ((A == null || A.length == 0) &&
            (B == null || B.length == 0)) {
            return -1.0;        
        }
        
        int m = (A == null) ? 0 : A.length;
        int n = (B == null) ? 0 : B.length;
        int len = m + n;
        
        // merge sort
        int i = 0, j = 0, k = 0;    // i for A, j for B, k for C
        int[] C = new int[len];
        // case1: both A and B have elements
        while (i < m && j < n) {
            if (A[i] < B[j]) {
                C[k++] = A[i++];
            } else {
                C[k++] = B[j++];
            }
        }
        // case2: only A has elements
        while (i < m) {
            C[k++] = A[i++];
        }
        // case3: only B has elements
        while (j < n) {
            C[k++] = B[j++];
        }
        
        // return median for even and odd cases
        if (len % 2 == 0) {
            return (C[(len - 1) / 2] + C[len / 2]) / 2.0;
        } else {
            return C[len / 2];
        }
    }
}
```



#### 二、二分查找

本题目可以转化为更一般的情况，寻找两个排序数组中第 `k` 大（从 `1` 开始数）的数， `k` 为两个数组长度和的中位数。如果每次比较都能舍弃 `k/2` 个数，那么时间复杂度就是对数线性级别。

我们来比较数组 `A` 和数组 `B` 的第 `k/2` 个数的大小，分为三种情况：

-  `A[k/2 - 1] == B[k/2 - 1]` ，不难得到此时两个数组第 `k` 大的数就是 `A[k/2 - 1]` 。
-  `A[k/2 - 1] < B[k/2 - 1]` ，那么在合并数组后 `A[0] … A[k/2 - 1]` 一定小于第 k 大的数，舍弃这一部分数，接着从 `A[k/2] … A[A.length - 1]` 中找。由于舍弃了 `k/2` 个数，所以第 `k` 大的数相应变成第 `k - k/2` 大的数。
-  `A[k/2 - 1] > B[k/2 - 1]` ，与上述类似。

几个边界条件的判断：

1. 当其中一个数组长度为 `0` 时，直接从另一个数组中取值即可。
2. 当 `k == 1` 时。

##### 算法复杂度

- 时间复杂度：`k = (m + n) / 2` ，所以复杂度为 `O(log(m + n))` 。
- 空间复杂度： `O(1)` 。

Java 实现

```java
class Solution {
    /**
     * @param A: An integer array.
     * @param B: An integer array.
     * @return: a double whose format is *.5 or *.0
     */
    public double findMedianSortedArrays(int[] A, int[] B) {
        int len = A.length + B.length;
        if (len % 2 == 1) {
            return findKth(A, 0, B, 0, len / 2 + 1);
        }
        return (findKth(A, 0, B, 0, len / 2) + findKth(A, 0, B, 0, len / 2 + 1)) / 2.0;
    }
    
    // find kth number of two sorted array
    public static int findKth(int[] A, int A_start,
                                int[] B, int B_start,
                                int k) {
        if (A_start >= A.length) {
            return B[B_start + k - 1];
        }                             
        if (B_start >= B.length) {
            return A[A_start + k - 1];
        }
        
        if (k == 1) {
            return Math.min(A[A_start], B[B_start]);
        }
        
        int A_key = A_start + k / 2 - 1 < A.length
                    ? A[A_start + k / 2 - 1]
                    : Integer.MAX_VALUE;
        int B_key = B_start + k / 2 - 1 < B.length
                    ? B[B_start + k / 2 - 1]
                    : Integer.MAX_VALUE;
        
        if (A_key < B_key) {
            return findKth(A, A_start + k / 2, B, B_start, k - k / 2);
        } else {
            return findKth(A, A_start, B, B_start + k / 2, k - k / 2);
        }
    }
}

```



### 参考

1. [Median of two Sorted Arrays | 九章算法](http://www.jiuzhang.com/solutions/median-of-two-sorted-arrays/)
2. [Median of two Sorted Arrays | 数据结构与算法/leetcode/lintcode题解](https://algorithm.yuanbin.me/zh-hans/binary_search/median_of_two_sorted_arrays.html)
3. [Median of Two Sorted Array leetcode java | 爱做饭的小莹子](http://www.cnblogs.com/springfor/p/3861890.html)