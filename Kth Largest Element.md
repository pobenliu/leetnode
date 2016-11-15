#  Kth Largest Element

 Kth Largest Element  ( [leetcode]()  [lintcode]() )

```
Description
Find K-th largest element in an array.

Notice
You can swap elements in the array

Example
In array [9,3,2,4,8], the 3rd largest element is 4.
In array [1,2,3,4,5], the 1st largest element is 5, 2nd largest element is 4, 
3rd largest element is 3 and etc.

Challenge 
O(n) time, O(1) extra memory.
```

### 解题思路

参考题目 Median 的思路。

Java 实现

```java
class Solution {
    /*
     * @param k : description of k
     * @param nums : array of nums
     * @return: description of return
     */
    public int kthLargestElement(int k, int[] nums) {
        if (nums == null || nums.length == 0 
            || k <= 0 || k > nums.length) {
            return -1;
        }
        
        return helper(nums, 0, nums.length - 1, k - 1);
    }
    
    private int helper(int[] nums, int start, int end, int k) {
        if (start >= end) {
            return nums[end];
        }
        
        int m = start;
        for (int i = start + 1; i < end + 1; i++) {
            if (nums[i] > nums[start]) {
                m++;
                swap(nums, i, m);
            }
        }
        swap(nums, m, start);
        
        if (m == k) {
            return nums[m];
        } else if (m > k) {
            return helper(nums, start, m - 1, k);
        } else {
            return helper(nums, m + 1, end, k);
        }
    }
    
    private void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
};
```

