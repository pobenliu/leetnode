#  Two Sum II

 Two Sum II ( [lintcode](http://www.lintcode.com/en/problem/two-sum-ii/) )

```
Description
Given an array of integers, find how many pairs in the array such that their sum is bigger than a specific target number. 
Please return the number of pairs.

Example
Given numbers = [2, 7, 11, 15], target = 24. Return 1. (11 + 15 is the only pair)

Challenge 
Do it in O(1) extra space and O(nlogn) time.
```

#### 解题思路

使用对撞型双指针，考虑到原始数组可能是乱序，需要先对数组进行排序。

```java
public class Solution {
    /**
     * @param nums: an array of integer
     * @param target: an integer
     * @return: an integer
     */
    public int twoSum2(int[] nums, int target) {
        // Write your code here
        if (nums == null || nums.length < 2) {
            return 0;
        }
        Arrays.sort(nums);
        
        int ans = 0;
        int start = 0; 
        int end = nums.length - 1;
        while (start < end) {
            int sum = nums[start] + nums[end];
            if (sum > target) {
                ans += end - start;
                end--;
            } else {
                start++;
            }
        }
        return ans;
    }
}
```

