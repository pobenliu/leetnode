# First Position of Target

 First Position of Target ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/first-position-of-target/) )

```
For a given sorted array (ascending order) and a target number, 
find the first index of this number in O(log n) time complexity.
If the target number does not exist in the array, return -1.

Example
If the array is [1, 2, 3, 3, 4, 5, 10], for given target 3, return 2.

Challenge
If the count of numbers is bigger than 2^32, can your code work properly?
```

### 解题思路

数组中有重复元素，在二分查找确定目标值第一次出现位置时，中间元素 `nums[mid] == target` ，有可能左半部分还有相同元素，所以此时应该把 `end` 位置移动到 `mid` 。

##### 算法复杂度：

- 时间复杂度： `O(log n)` 。
- 空间复杂度： `O(1)` 。

Java 实现

```java
class Solution {
    /**
     * @param nums: The integer array.
     * @param target: Target to find.
     * @return: The first position of target. Position starts from 0.
     */
    public int binarySearch(int[] nums, int target) {
        //write your code here
        if (nums == null || nums.length == 0){
            return -1;
        }

        int start = 0, end = nums.length - 1;
        while(start + 1 < end) {
            int mid =  start + (end - start)/2;
            if (nums[mid] == target) {
                end = mid;
            } else if (nums[mid] < target) {
                start = mid;
            } else if (nums[mid] > target) {
                end =  mid;
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

​