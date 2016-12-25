#  House Robber II

```
Description
After robbing those houses on that street, 
the thief has found himself a new place for his thievery so that he will not get too much attention. 
This time, all houses at this place are arranged in a circle. 
That means the first house is the neighbor of the last one. 
Meanwhile, the security system for these houses remain the same as for those in the previous street.

Given a list of non-negative integers representing the amount of money of each house, 
determine the maximum amount of money you can rob tonight without alerting the police.

Notice
This is an extension of House Robber.
```

### 解题思路

参考 House Robber 的思路，如果房子围城一个圈，那么偷第一个房子，就不能偷最后一个房子，可以进一步分解问题。

```java
public class Solution {
    /**
     * @param nums: An array of non-negative integers.
     * return: The maximum amount of money you can rob tonight
     */
    public int houseRobber2(int[] nums) {
        // write your code here
        if (nums == null || nums.length == 0) {
            return 0;
        }
        if (nums.length == 1) {
            return nums[0];
        }
        
        int ans1 = houseRobber1(nums, 0, nums.length - 2);
        int ans2 = houseRobber1(nums, 1, nums.length - 1);
        
        return Math.max(ans1, ans2);
    }
    
    private int houseRobber1(int[] A, int start, int end) {
        if (start == end) {
            return A[start];
        }
        int[] f = new int[A.length + 1];
        f[start] = A[start];
        f[start + 1] = Math.max(A[start + 1], f[start]);
        
        for (int i = start + 2; i <= end; i++) {
            f[i] = Math.max(f[i-1], f[i-2] + A[i]);
        }
        return f[end];
    }
}
```

可以使用滚动数组优化空间

```java
public class Solution {
    /**
     * @param nums: An array of non-negative integers.
     * return: The maximum amount of money you can rob tonight
     */
    public int houseRobber2(int[] nums) {
        // write your code here
        if (nums == null || nums.length == 0) {
            return 0;
        }
        if (nums.length == 1) {
            return nums[0];
        }
        
        int ans1 = houseRobber1(nums, 0, nums.length - 2);
        int ans2 = houseRobber1(nums, 1, nums.length - 1);
        
        return Math.max(ans1, ans2);
    }
    
    private int houseRobber1(int[] A, int start, int end) {
        if (start == end) {
            return A[start];
        }
        int[] f = new int[2];
        f[start % 2] = A[start];
        f[(start + 1) % 2] = Math.max(A[start + 1], f[start]);
        
        for (int i = start + 2; i <= end; i++) {
            f[i % 2] = Math.max(f[(i-1) % 2], f[(i-2) % 2] + A[i]);
        }
        return f[end % 2];
    }
}
```



