# Search Insert Position

Search Insert Position  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/search-insert-position/) )

```
Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.
You may assume NO duplicates in the array.

Example
[1,3,5,6], 5 → 2
[1,3,5,6], 2 → 1
[1,3,5,6], 7 → 4
[1,3,5,6], 0 → 0

Challenge
O(log(n)) time
```



### 解题思路

该题目换一个说法，就是要寻找第一个大于等于 `target` 的位置。插入元素和查找元素的区别在于，可能插入到最后一个位置的后面，此时 `target > A[n - 1]` 。

Java 实现

```java
public class Solution {
    public int searchInsert(int[] A, int target) {
        if (A == null || A.length == 0) {
            return 0;
        }
        int start = 0, end = A.length - 1;

        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (A[mid] == target) {
                return mid;
            } else if (A[mid] < target) {
                start = mid;
            } else {
                end = mid;
            }
        }

        if (A[start] >= target) {
            return start;
        } else if (A[end] >= target) {
            return end;
        } else {
            return end + 1;    
        }
    }
}
```



另一种实现方法

```java
public class Solution {
    /**
     * param A : an integer sorted array
     * param target :  an integer to be inserted
     * return : an integer
     */
    public int searchInsert(int[] A, int target) {
        // write your code here
        if (A == null || A.length == 0) {
            return 0;        //留意此题的边界条件，此处其实也是插入元素和查找元素的不同
        }
        int start = 0, end = A.length - 1;
        if (A[end] < target) {
            return end + 1;
        }
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (A[mid] == target){
                return mid;
            }else if (A[mid] < target) {
                start = mid;
            }else if (A[mid] > target) {
                end = mid;
            }
        }
        if (A[start] >= target) {
            return start;
        }
        if (A[end] >= target) {
            return end;
        }
        return -1;
    }
}
```



### 参考

1. [Search Insert Position | 九章算法](http://www.jiuzhang.com/solutions/search-insert-position/)

   ​