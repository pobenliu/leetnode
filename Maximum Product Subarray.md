#  Maximum Product Subarray

 Maximum Product Subarray  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/maximum-product-subarray/) )

```
Description
Find the contiguous subarray within an array (containing at least one number) which has the largest product.

Example
For example, given the array [2,3,-2,4], the contiguous subarray [2,3] has the largest product = 6.
```

### 解题思路

可参考题目 Maximum Subarray，不过有一点需要注意的是，乘积有正有负，负负得正，所以在本题中除了记录当前的乘积最大值外，还需要记录当前的乘积最小值。

1. 定义状态：定义一维状态变量 `curtMax[i]` 和 `curtMin[i]` ，表示以第 `i` 个数字 `nums[i - 1]` 为结尾的子数组的最大乘积和最小乘积，定义变量 `max` 存储每次循环计算后的最大值。
2. 定义状态转移函数：
   - 对于 `curtMax[i]`，它应该是 `curtMax[i - 1] * nums[i - 1], curtMin[i - 1] * nums[i - 1], nums[i - 1]` 三者之中的最大值。
   - 对于 `curtMin[i]`，它应该是 `curtMax[i - 1] * nums[i - 1], curtMin[i - 1] * nums[i - 1], nums[i - 1]` 三者之中的最小值。
   - 每次循环记录当前得到的最大乘积值为 `max`。
   - 根据状态转移函数，当前状态只取决于上一个状态，所以使用滚动数组进行内存优化。
3. 定义起点：三者的起点都为 `nums[0]`。
4. 定义终点：最终结果即为 `max`。

```java
public class Solution {
    /**
     * @param nums: an array of integers
     * @return: an integer
     */
    public int maxProduct(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        int[] curtMinProduct = new int[2];
        int[] curtMaxProduct = new int[2];
        curtMinProduct[0] = nums[0];
        curtMaxProduct[0] = nums[0];
        int maxProduct = nums[0];
        
        for (int i = 1; i < nums.length; i++) {
            curtMinProduct[i % 2] = Math.min(nums[i],
                                Math.min(curtMinProduct[(i - 1) % 2] * nums[i], curtMaxProduct[(i - 1) % 2] * nums[i]));
            curtMaxProduct[i % 2] = Math.max(nums[i],
                                Math.max(curtMinProduct[(i - 1) % 2] * nums[i], curtMaxProduct[(i - 1) % 2] * nums[i]));
            maxProduct = Math.max(maxProduct, curtMaxProduct[i % 2]);
        }
        
        return maxProduct;
    }
}
```

