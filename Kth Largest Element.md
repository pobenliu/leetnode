# Kth Largest Element

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
        // write your code here
        if (nums == null || nums.length == 0 || 
            k <= 0 || k > nums.length) {
            return -1;        
        }
        
        return helper(nums, 0, nums.length - 1, k - 1);
    }
    
    private int helper(int[] nums, int start, int end, int k) {
        if (start >= end) {
            return nums[end];
        }
        
        int prev = start;
        int curt = start + 1;
        int pivot = nums[start];
        for( ; curt <= end; curt++) {
            if (nums[curt] > pivot) {
                prev++;
                swap(nums, prev, curt);
            }
        }
        swap(nums, prev, start);
        if (prev == k) {
            return nums[prev];
        } else if (prev > k) {
            return helper(nums, start, prev - 1, k);
        } else {
            return helper(nums, prev + 1, end, k);
        }
    }
    
    private void swap(int[] A, int i, int j) {
        int temp = A[i];
        A[i] = A[j];
        A[j] = temp;
    }
};
```

