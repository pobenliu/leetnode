# Partition Array

 Partition Array  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/partition-array/) )

```
Description
Given an array nums of integers and an int k, partition the array 
(i.e move the elements in "nums") such that:
All elements < k are moved to the left
All elements >= k are moved to the right
Return the partitioning index, i.e the first index i nums[i] >= k.

Notice
You should do really partition in array nums instead of 
just counting the numbers of integers smaller than k.
If all elements in nums are smaller than k, then return nums.length

Example
If nums = [3,2,2,1] and k=2, a valid answer is 1.

Challenge 
Can you partition the array in-place and in O(n)?
```

### 解题思路

#### 一、反方向双指针

本题目和快速排序是同一类题目，使用指向首尾的两个指针 `left` 和 `right` 向中间遍历，当 `left` 指向对象大于等于 `k` ，`right` 指向对象小于 `k` 时，交换两者，直至 `left > right` 。操作过程要时刻注意两个指针的相对位置，确保不越界。

##### 易错点

> 1. 在移动过程要确保两根指针的相对位置，不会让数组越界。
> 2. 两个特殊情况需考虑，数组全部小于 `k`，数组全部大于 `k`。

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



#### 二、同方向双指针

使用指针 `prev` 记录最后一个小于 `k` 的元素索引，`curt` 遍历数组寻找小于 `k` 的元素。

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
        int prev = -1;
        int curt = 0;
        for ( ; curt < nums.length; curt++) {
            if (nums[curt] < k) {
                prev++;
                swap(nums, prev, curt);
            } 
        }
	    
	    return prev + 1;
    }
    
    private void swap(int[] nums, int start, int end) {
        int tmp = nums[start];
        nums[start] = nums[end];
        nums[end] = tmp;
    }
}
```

