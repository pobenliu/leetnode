#  Partition Array

 Partition Array  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/partition-array/) )

```
Description
Given an array nums of integers and an int k, partition the array (i.e move the elements in "nums") such that:
All elements < k are moved to the left
All elements >= k are moved to the right
Return the partitioning index, i.e the first index i nums[i] >= k.

Notice
You should do really partition in array nums instead of just counting the numbers of integers smaller than k.
If all elements in nums are smaller than k, then return nums.length

Example
If nums = [3,2,2,1] and k=2, a valid answer is 1.

Challenge 
Can you partition the array in-place and in O(n)?
```

### 解题思路

本题目和快速排序是同一类题目，使用指向首尾的两个指针 `left` 和 `right` 向中间遍历，当 `left` 指向对象大于等于 `k` ，`right` 指向对象小于 `k` 时，交换两者，直至 `left > right` 。操作过程要时刻注意两个指针的相对位置，确保不越界。

Java 实现

```java
public class Solution {
	/** 
     *@param nums: The integer array you should partition
     *@param k: As description
     *return: The index after partition
     */
    public int partitionArray(int[] nums, int k) {
	    if (nums == null || nums.length == 0) {
	        return 0;
	    }
	    
	    int left = 0;
	    int right = nums.length - 1;
	    
	    while (left <= right) {
	        while (left <= right && nums[left] < k) {
	            left++;
	        }
	        while (left <= right && nums[right] >= k) {
	            right--;
	        }
	        
	        if (left <= right) {
	            int temp = nums[left];
	            nums[left] = nums[right];
	            nums[right] = temp;
	            left++;
	            right--;
	        }
	    }
	    
        return left;
    }
}
```



### 参考

