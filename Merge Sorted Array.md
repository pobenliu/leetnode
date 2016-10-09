# Merge Sorted Array

 Merge Sorted Array  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/merge-sorted-array/#) )

```
Description
Given two sorted integer arrays A and B, merge B into A as one sorted array.

Notice
You may assume that A has enough space (size that is greater or equal to m + n) to hold additional elements from B. 
The number of elements initialized in A and B are m and n respectively.

Example
A = [1, 2, 3, empty, empty], B = [4, 5]
After merge, A will be filled as [1, 2, 3, 4, 5]
```

### 解题思路

如果从头开始比较，在合并过程中数组 A 会涉及大量的移动操作。考虑到数组 A 中有足够的空间，可以从数组末段开始比较，以从大到小的顺序合并两个数组。需要注意 A 中原有元素可能已全部移至尾部， B 中还有元素。

##### 算法复杂度：

- 时间复杂度： `O(m + n)` 。
- 空间复杂度： `O(1)` 。

Java 实现

```java
class Solution {
    /**
     * @param A: sorted integer array A which has m elements, 
     *           but size of A is m+n
     * @param B: sorted integer array B which has n elements
     * @return: void
     */
    public void mergeSortedArray(int[] A, int m, int[] B, int n) {
        if (A == null || A.length == 0 ||
            B == null || B.length == 0) {
            return;        
        }
        
        int i = m - 1;
        int j = n - 1;
        int k = m + n - 1;
        // compare the elements in A and B from the bottom
        while (i >= 0 && j >= 0) {
            if (A[i] > B[j]) {
                A[k--] = A[i--];
            } else {
                A[k--] = B[j--];
            }
        }
        // elements in A have been moved to bottom while B still has elements
        while (j >= 0) {
            A[k--] = B[j--];
        }
    }
}
```



### 参考

1. [Merge Sorted Array | 九章算法](http://www.jiuzhang.com/solutions/merge-sorted-array/)