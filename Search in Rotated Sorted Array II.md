# Search in Rotated Sorted Array II

 Search in Rotated Sorted Array II  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/search-in-rotated-sorted-array-ii/#) )

```
Description
Follow up for Search in Rotated Sorted Array:
What if duplicates are allowed?
Would this affect the run-time complexity? How and why?
Write a function to determine if a given target is in the array.

Example
Given [1, 1, 0, 1, 1, 1] and target = 0, return true.
Given [1, 1, 1, 1, 1, 1] and target = 0, return false.
```

### 解题思路

有重复元素，那么考虑一个极端的情况就是数组 `[0, 1, 1, 1, 1, … , 1]` 的旋转数组，该情况无法使用二分法，需要遍历所有元素，时间复杂度为 `O(n)` ，这是最坏情况，可以考虑使用直接遍历。

Java 实现

```java
public class Solution {
    /** 
     * param A : an integer ratated sorted array and duplicates are allowed
     * param target :  an integer to be search
     * return : a boolean 
     */
    public boolean search(int[] A, int target) {
        // write your code here
        if (A == null || A.length == 0 ) {
            return false;
        }
        for (int i = 0; i < A.length; i++) {
            if (A[i] == target) {
                return true;
            }
        }
        return false;
    }
}
```

当然，其实我们大可不必放弃治疗。考虑到最坏情况不可能一直出现，针对一般情况（只有部分重复元素），我们可以将二分查找和遍历结合起来。

Java 实现

```java
public class Solution {
    /** 
     * param A : an integer ratated sorted array and duplicates are allowed
     * param target :  an integer to be search
     * return : a boolean 
     */
    public boolean search(int[] A, int target) {
        if (A == null || A.length == 0) {
            return false;
        }
        
        int start = 0, end = A.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (A[mid] == target) {
                return true;
            }
            if (A[mid] > A[start]) {
                if (A[start] <= target && target <= A[mid]) {
                    end = mid;
                } else {
                    start = mid;
                }
            } else if (A[mid] < A[start]) {
                if (A[mid] <= target && target <= A[end]) {
                    start = mid;
                } else {
                    end = mid;
                }
            } else {
                start++;	// A[start] == A[mid], then skip duplicate one
            }
        }
        
        if (A[start] == target) {
            return true;
        } else if (A[end] == target) {
            return true;
        }
        return false;
    }
}
```



### 参考

1. [Search in Rotated Sorted Array II | 九章算法](http://www.jiuzhang.com/solutions/search-in-rotated-sorted-array-ii/)
2. [[LeetCode] Search in Rotated Sorted Array II 解题报告 | 水中的鱼](http://fisherlei.blogspot.jp/2013/01/leetcode-search-in-rotated-sorted-array_3.html)