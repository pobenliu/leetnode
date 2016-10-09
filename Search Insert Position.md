# Search Insert Position

Search Insert Position  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/search-insert-position/) )

```
Given a sorted array and a target value, return the index if the target is found. 
If not, return the index where it would be if it were inserted in order.
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

该题目变换一下说法，可以是：

- 寻找第一个大于等于 `target` 的位置。
- 寻找最后一个小于等于 `target` 的位置。

此外，插入元素和查找元素的区别在于，可能插入到数组最后一个位置的后面，此时对应 `target > A[n - 1]` （目标值大于数组最大值）。

##### 算法复杂度：

- 时间复杂度： `O(log n)` 。
- 空间复杂度： `O(1)` 。

##### 易错点：

> 1. 在判断边界条件时，如果数组为空，或者不为空但是不含任何元素，那么插入位置就是 0 。
> 2. 记得目标值大于数组中所有元素这种情况！

Java 实现

```java
public class Solution {
    public int searchInsert(int[] A, int target) {
        if (A == null || A.length == 0) {
            return 0;	// if there is no elements in array A, then insert position 0
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
        
        if (A == null || A.length == 0) {
            return 0;        // if there is no elements in array A, then insert position 0
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