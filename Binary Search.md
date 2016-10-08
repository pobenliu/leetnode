# Binary Search

Binary Search

```
Binary search is a famous question in algorithm. 
For a given sorted array (ascending order) and a target number, find the first index of this number in O(log n) time complexity. 
If the target number does not exist in the array, return -1. 

Example 
If the array is [1, 2, 3, 3, 4, 5, 10], for given target 3, return 2.
```

### 解题思路

这里参考了九章算法 `binary search` 的解题模板：

- `start + 1 < end` 
- `mid = start + (end - start) / 2`
- `A[mid] ==, <, >`
- `A[start] A[end] ? target`

注：

1. 写为 `start + 1 < end` 主要是为了防止出现死循环。如果写成 `start＜end`，那么当 `start = 1`，`end = 2` 时， `mid = start + (end - start)/2 = 1` ，如果在判决条件下得到 `start = mid;` ，会出现死循环。
2. 之所以不使用 `mid = (start + end ) / 2` ，是为了防止`start`和`end`都很大的时候溢出。 

Java 实现

```java
public class Solution {
    /**
     * @param A an integer array sorted in ascending order
     * @param target an integer
     * @return an integer
     */
    public int findPosition(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return -1;
        }
        
        int start = 0, end = nums.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                start = mid;
            } else {
                end = mid;
            }
        }
        
        if (nums[start] == target) {
            return start;
        }
        if (nums[end] == target) {
            return end;
        }
        return -1;
    }
}
```



另一种常规的实现方法，注意比较 `start` 和 `end` 在移动时与第一种方法的区别：

```java
public class Solution {
    /**
     * @param A an integer array sorted in ascending order
     * @param target an integer
     * @return an integer
     */
    public int findPosition(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return -1;
        }
        
        int start = 0, end = nums.length - 1;
        while (start < end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                start = mid + 1;
            } else {
                end = mid - 1;
            }
        }
        
        if (nums[start] == target) {
            return start;
        }
        return -1;
    }
}
```



### 参考

1. [Binary Search | 九章算法](http://www.jiuzhang.com/solutions/binary-search/) 